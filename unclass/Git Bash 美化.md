在用户根目录下创建一个 `minttyrc` 文件
```shell
FontHeight=10
Transparency=low
FontSmoothing=full
Locale=zh_CN
Charset=UTF-8
Columns=88
Rows=26
OpaqueWhenFocused=no
Scrollbar=none
Language=zh_CN
ForegroundColour=255,255,255
BackgroundColour=0,43,54
CursorColour=220,130,71
BoldBlack=128,128,128
Green=64,200,64
BoldGreen=64,255,64
Yellow=190,190,0
BoldYellow=255,255,64
Blue=135,144,255
BoldBlue=30,144,255
Magenta=211,54,130
BoldMagenta=255,128,255
Cyan=64,190,190
BoldCyan=128,255,255
White=250,240,230
BoldWhite=250,240,230
BellTaskbar=no
Term=xterm-256color
FontWeight=400
FontIsBold=no
BellType=0
CtrlShiftShortcuts=yes
ConfirmExit=no
AllowBlinking=yes
BoldAsFont=no
```

在 Git Bash 安装目录的 `/etc/profile.d/git-prompt.sh` 文件
```shell
if test -f /etc/profile.d/git-sdk.sh
then
    TITLEPREFIX=SDK-${MSYSTEM#MINGW}
else
    TITLEPREFIX=$MSYSTEM
fi
if test -f ~/.config/git/git-prompt.sh
then
    . ~/.config/git/git-prompt.sh
else
    PS1='\[\033]0;\w\007\]'  # 窗口标题
    PS1="$PS1"'\n'                   # 换行
    PS1="$PS1"'\[\033[32;1m\]'       # 绿色
    PS1="$PS1"'➜ '
    PS1="$PS1"'\u '                             # 用户名
    PS1="$PS1"'\[\033[35m\]'             # 粉红色
    PS1="$PS1"'\t '  
    PS1="$PS1"'\[\033[33;1m\]'       # 黄色
    PS1="$PS1"'\W'                   # 当前目录
    if test -z "$WINELOADERNOEXEC"
    then
        GIT_EXEC_PATH="$(git --exec-path 2>/dev/null)"
        COMPLETION_PATH="${GIT_EXEC_PATH%/libexec/git-core}"
        COMPLETION_PATH="${COMPLETION_PATH%/lib/git-core}"
        COMPLETION_PATH="$COMPLETION_PATH/share/git/completion"
        if test -f "$COMPLETION_PATH/git-prompt.sh"
        then
            . "$COMPLETION_PATH/git-completion.bash"
            . "$COMPLETION_PATH/git-prompt.sh"
            PS1="$PS1"'\[\033[36m\]'  # 青色
            PS1="$PS1"'`__git_ps1`'   # bash function
        fi
    fi
    PS1="$PS1"'\[\033[0m\] '      # 白色
fi
MSYS2_PS1="$PS1"               # for detection by MSYS2 SDK's bash.basrc
```

参考:
1. [git 美化 - 鮮紅之淚 の blog](https://www.scarletdrop.cn/archives/9)
2. [Fetching Title#jegj](https://github.com/xnng/my-git-bash)