FUNCTION LoginFlow():
    START
    
    // --- Row 1 ---
    Display Login Form
    User Chooses Favourite Login Method
    Display Form According to login method
    
    LABEL UsertEnterCredentials:
    Usert Enter Credentials
    
    IF NOT (Valid Credentials Format?) THEN
        Display Error Message to Enter Correct Data
        GOTO UsertEnterCredentials
    ELSE
        Save Credentials
        Display favourite verify channel
        Choose favorite verification channel
        Verification Method + Credentials
        GOTO SendOTP
    ENDIF

    // --- Row 2 & 3 ---
    LABEL SendOTP:
    Send OTP to The Verification Method
    Verification Sender (Subprocess)
    OTP Saved
    
    IF NOT (OTP Sent Correctly?) THEN
        Display Error Message
        // Flows back to selection
        GOTO DisplayFavouriteVerifyChannel 
    ELSE
        GOTO DisplayFormToEnterOTP
    ENDIF

    LABEL DisplayFavouriteVerifyChannel:
    Display favourite verify channel
    Choose favorite verification channel
    Verification Method + Credentials
    GOTO SendOTP

    // --- Row 4 ---
    LABEL DisplayFormToEnterOTP:
    Display Form To Enter OTP
    User Enters OTP
    
    IF (Correct OTP?) THEN
        Fetch User Data
        Query Database
        GOTO CheckUserExistence
    ELSE
        // Note: Diagram does not explicitly map the 'No' path for Correct OTP?
        // Assumed block state or loop based on visual flow continuity
    ENDIF

    // --- Row 5 ---
    LABEL CheckUserExistence:
    User Data Saved
    
    IF NOT (User Exist?) THEN
        Registeration Process (Subprocess)
        GOTO EndProcess
    ELSE
        GOTO CheckAlreadyBlocked
    ENDIF

    // --- Row 6, 7 & 8 ---
    LABEL CheckAlreadyBlocked:
    IF (Is Already Blocked?) THEN
        GOTO DisplayTimerForUserToWait
    ELSE
        IF (Is Valid Credentials?) THEN
            Authorize User
            Save last successful login + reset retry counter (Write to Database)
            Display System for User.
            GOTO EndProcess
        ELSE
            Audit failing retry (Write to Database)
            
            IF (is failing for more than three times?) THEN
                Block user for specific period based on the attempt number
                GOTO DisplayTimerForUserToWait
            ELSE
                Display "Invalid User Name or Password"
                GOTO EndProcess
            ENDIF
        ENDIF
    ENDIF

    LABEL DisplayTimerForUserToWait:
    Display Timer for user to wait
    GOTO EndProcess

    LABEL EndProcess:
    END