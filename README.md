## 傻逼草泥马 install docker and docker compse
# 0) 卸载可能的旧包（安全起见）
sudo apt-get remove -y docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc || true

# 1) 依赖
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg

# 2) 准备 keyring 并导入官方 GPG key
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
  | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# 3) 写入 Docker 官方 apt 仓库（自动用系统代号）
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release; echo $VERSION_CODENAME) stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 4) 安装 Docker Engine + Buildx + Compose 插件
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 5) 开机自启并立即启动
sudo systemctl enable --now docker

# 6)（可选）让当前用户免 sudo 用 docker（重新登录生效）
sudo usermod -aG docker $USER

# 7) 验证
docker --version
docker compose version
sudo docker run --rm hello-world



# 运行nginx和portainer

mkdir -p nginx/html
cat > nginx/html/index.html <<'EOF'
<!doctype html>
<html lang="en"><head><meta charset="utf-8"><title>It works</title></head>
<body><h1>Hello from Nginx in Docker!</h1></body></html>
EOF

# 拉起
docker compose up -d
