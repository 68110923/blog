# 🔑 ssv — SSH 快捷登录

## 1. 安装 sshpass

```bash
brew install sshpass
```

## 2. 配置 ssv 命令

编辑 `~/.zshrc`，在末尾粘贴：

```shell
# ssv — 服务器快捷登录，支持别名和序号
ssv() {
  local conf="$HOME/.config/ssv/servers.conf"
  local lconf="${PWD}/servers.conf"
  local files=""
  [ -f "$lconf" ] && files="$lconf"
  [ -f "$conf" ] && files="${files:+$files }$conf"

  [ -z "$files" ] && { echo "❌ 未找到配置文件"; return 1; }

  local _n=() _e=() _u=() _p=()
  for f in $files; do
    while IFS=' ' read -r a b c d; do
      [ -z "$a" ] || [ "${a#\#}" != "$a" ] && continue
      _n+=("$a"); _e+=("$b"); _u+=("$c"); _p+=("$d")
    done < "$f"
  done

  local cnt=${#_n[@]}

  if [ $# -eq 0 ]; then
    echo "用法: ssv <别名|序号>"
    local i=1; while [ $i -le $cnt ]; do
      echo "  [$i] $_n[$i]  → $_u[$i]@${_e[$i]%%:*}:${_e[$i]##*:}"
      i=$((i+1))
    done
    return
  fi

  local key=$1; local idx=0

  if [ "$key" -eq "$key" ] 2>/dev/null; then
    [ "$key" -lt 1 ] || [ "$key" -gt "$cnt" ] && { echo "❌ 序号超出范围 (1~$cnt)"; return 1; }
    idx=$key
  else
    local i=1; while [ $i -le $cnt ]; do
      [ "$_n[$i]" = "$key" ] && { idx=$i; break; }
      i=$((i+1))
    done
    [ "$idx" -eq 0 ] && { echo "❌ 未知服务器: $key"; return 1; }
  fi

  echo "  → $_u[$idx]@${_e[$idx]%%:*}:${_e[$idx]##*:}"
  sshpass -p "$_p[$idx]" ssh -o StrictHostKeyChecking=no \
    -p "${_e[$idx]##*:}" "$_u[$idx]@${_e[$idx]%%:*}"
}
```

重载配置：

```bash
source ~/.zshrc
```

## 3. 配置服务器

**全局配置（所有目录生效）：**

```bash
vim ~/.config/ssv/servers.conf
```

```conf
# 格式: 别名 IP:端口 用户名 密码
主服务器  127.0.0.1:22  root  password
ys12      127.0.0.1:22  root  password
```

**当前目录配置（仅该项目生效）：**
在项目目录下创建 `servers.conf`，格式同上。`ssv` 会自动合并显示。

## 4. 使用

```bash
ssv           # 列出所有服务器
ssv 1         # 按序号登录
ssv 主服务器  # 按别名登录
```
