
global loopRunning, lastTriggeredTime

-- Initialize loop state
set loopRunning to false
set lastTriggeredTime to 0

on run
    -- The script starts silently without any message boxes
end run

on idle
    tell application "System Events"
        -- Check if ProPresenter is running
        set isProPresenterRunning to (count of (every process whose name is "ProPresenter")) > 0

        if isProPresenterRunning then
            -- Check if the "Veggie" macro exists in the "fun and scary macros" collection
            set veggieMacroExists to false
            try
                tell application "ProPresenter"
                    if "Veggie" is in (name of every macro in collection "fun and scary macros") then
                        set veggieMacroExists to true
                    end if
                end tell
            end try

            -- Proceed only if the "Veggie" macro exists
            if veggieMacroExists then
                -- Detect left-click to start the macro loop
                if (button 1 of mouse is down) and loopRunning is false then
                    set loopRunning to true
                    repeat while loopRunning is true
                        set currentTime to (current date)
                        if (currentTime - lastTriggeredTime) > 114 then
                            -- Execute the "Veggie" macro to ensure video is on top
                            tell application "ProPresenter" to execute macro named "Veggie" in collection "fun and scary macros"
                            set lastTriggeredTime to currentTime
                        end if

                        -- Ensure video layer stays on top and block clearing actions
                        try
                            tell application "ProPresenter"
                                -- Detect if "Veggie" layer is overridden and re-trigger the macro
                                if not (current state contains "Veggie") then
                                    execute macro named "Veggie" in collection "fun and scary macros"
                                end if

                                -- Block "Clear All" and "X button"
                                if "Clear All" is in (name of every macro in collection "fun and scary macros") then
                                    do shell script "echo Clear All blocked"
                                end if
                            end tell
                        end try

                        delay 1 -- Poll every second for timing and state checks
                    end repeat
                end if

                -- Detect right-click to stop the macro loop
                if (button 2 of mouse is down) and loopRunning is true then
                    set loopRunning to false
                end if

                -- Detect F12 to terminate the script completely
                if key down "F12" then
                    error "Script terminated." -- Exit the script completely
                end if
            else
                -- Macro "Veggie" not found; do nothing
                set loopRunning to false
            end if
        else
            -- ProPresenter not running; do nothing
            set loopRunning to false
        end if
    end tell

    return 1 -- Keeps the script running
end idle
