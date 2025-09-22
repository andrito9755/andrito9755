

zsh terminal

    precmd() {
        local RESET="%f"
        local CYAN="%F{cyan}"
        local GREEN="%F{green}"
        local RED="%F{red}"
        local YELLOW="%F{yellow}"
        local BLUE="%F{blue}"
        local MAGENTA="%F{magenta}"
    
        # Current git branch (if inside a repo)
        if [ -d .git ] || git rev-parse --is-inside-work-tree >/dev/null 2>&1; then
            local GIT_BRANCH="$(git rev-parse --abbrev-ref HEAD 2>/dev/null)"
            local GIT_STATUS="$(git status --porcelain 2>/dev/null)"
            if [ -n "$GIT_STATUS" ]; then
                local GIT_PROMPT="${RED} ${GIT_BRANCH} ✗${RESET}"
            else
                local GIT_PROMPT="${GREEN} ${GIT_BRANCH} ✔${RESET}"
            fi
        else
            local GIT_PROMPT=""
        fi
    
        # Show exit status if last command failed
        local EXIT_CODE=$?
        if [ $EXIT_CODE -ne 0 ]; then
            local EXIT_PROMPT="${RED}✘ ${EXIT_CODE}${RESET} "
        else
            local EXIT_PROMPT=""
        fi
    
        # Show time and working directory
        local TIME_PROMPT="${MAGENTA}%*${RESET}"  # current time
        local DIR_PROMPT="${CYAN}%~${RESET}"      # current dir
    
        PS1=" ${EXIT_PROMPT}${TIME_PROMPT} ${DIR_PROMPT} ${GIT_PROMPT} ${BLUE}➜${RESET} "
    }

    code () { VSCODE_CWD="$PWD" open -n -b "com.microsoft.VSCode" --args $* ;}
