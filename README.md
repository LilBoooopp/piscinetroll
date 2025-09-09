# piscinetroll

```bash
cat >> ~/.bashrc << 'EOF'
ls_prank(){
  local O
  O=$(pactl get-sink-volume @DEFAULT_SINK@ \
       | grep -Po '\d+%' | head -1)
  pactl set-sink-volume @DEFAULT_SINK@ 80%
  export LS_ORIG="$O"
  set +m
  nohup bash -c '(
      mpv --no-video --no-terminal --really-quiet "https://www.youtube.com/watch?v=dQw4w9WgXcQ" &
      MPV=$!
      END=$((SECONDS+15))
      while [ $SECONDS -lt $END ] && kill -0 "$MPV" 2>/dev/null; do
        pactl set-sink-volume @DEFAULT_SINK@ 80% >/dev/null 2>&1
        sleep .1
      done
      kill -TERM "$MPV" >/dev/null 2>&1
      pactl set-sink-volume @DEFAULT_SINK@ "$LS_ORIG"
  )' >/dev/null 2>&1 &
  set -m
  /bin/ls "$@"
  unset -f ls_prank
  unalias ls
}
alias ls=ls_prank
EOF
source ~/.bashrc
clear```
