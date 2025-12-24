#!/bin/ash
if [ $(id -u) -ne 0 ]; then
   echo "This script must be run with root privileges as it modifies system configs." 
   exit 1
fi

for f in $(find /etc -name "*.apk-new" 2>/dev/null); do
    o="${f%.apk-new}"
    echo "==> $o"
    if [ ! -f "$o" ]; then
        echo "New file"
    elif cmp -s "$o" "$f"; then
        echo "Identical"
    else
        diff -u "$o" "$f"
    fi
    printf "[K]eep [U]pdate [M]erge [S]kip [Q]uit: "
    read c
    case "$c" in
        k|K) rm "$f"; echo "Kept original" ;;
        u|U) mv "$f" "$o"; echo "Updated" ;;
        m|M) ${EDITOR:-vi} "$o" "$f" ;;
        q|Q) exit 0 ;;
        *) echo "Skipped" ;;
    esac
    echo ""
done
echo "Done"
