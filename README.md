#!/bin/bash

# 确保使用 sudo
if [[ $EUID -ne 0 ]]; then
  echo "请使用 sudo 运行本脚本！"
  exit 1
fi

echo "[+] 正在安装官方仓库依赖..."

pacman -Syu --noconfirm \
hyprland waybar swww dunst wl-clipboard cliphist \
fcitx5 fcitx5-gtk fcitx5-configtool fcitx5-chinese-addons \
udiskie avizo \
alacritty kitty wezterm \
rofi ulauncher \
grim slurp brightnessctl pamixer \
swaylock-effects swayidle polkit-kde-agent \
git

echo "[+] 正在安装 AUR 依赖（需要 yay）..."

if ! command -v yay &>/dev/null; then
  echo "[!] 未检测到 yay，正在安装..."
  cd ~
  git clone https://aur.archlinux.org/yay.git
  cd yay
  makepkg -si --noconfirm
fi

yay -S --noconfirm \
anyrun hyprpm pyprland \
ttf-jetbrains-mono-nerd ttf-font-awesome

echo "[+] 克隆 Hyprland 配置..."

git clone https://github.com/0a00/hyprfiles.git ~/hyprfiles
mkdir -p ~/.config
cp -r ~/hyprfiles/config/hypr ~/.config/
chmod +x ~/.config/hypr/script/* 2>/dev/null

echo "[✓] 安装完成，请重启 Hyprland 或执行 'Hyprland' 以载入配置"
