FUNCTION RegistrationFlow():
    START
    
    // --- Row 1 ---
    Display Registration Options
    User Chooses Registration Option
    
    LABEL SpecificRegistrationForm:
    Display Specific Registration Form
    User Enters Registration Data
    GOTO ConnectorA

    // --- Row 2 ---
    LABEL ConnectorA:
    IF NOT (Valid Data Format?) THEN
        Display Error Message
        GOTO SpecificRegistrationForm
    ELSE
        Hold Entered Data
        Query DB for Similar Data
        
        IF (Same Identifying Data Exist?) THEN
            Inform User to Choose another input data (phone or mail)
            GOTO SpecificRegistrationForm
        ELSE
            GOTO ConnectorB
        ENDIF
    ENDIF

    // --- Row 3 ---
    LABEL ConnectorB:
    Send Verification Mail or message
    Inform User About Verification Code and Show Form
    
    LABEL ConnectorD:
    Wait until user input
    
    IF NOT (User Entered Data within Session Timeout Period?) THEN
        Wipe Entered Data
        GOTO SpecificRegistrationForm
    ELSE
        GOTO ConnectorC
    ENDIF

    // --- Row 4 ---
    LABEL ConnectorC:
    User Enters Verification Code
    
    IF (User Entered Correct Verification Code?) THEN
        Save User Data To Database (Write to Database)
        Authorize User
        GOTO EndProcess
    ELSE
        IF (Wrong Verification Code from More than 3 Times?) THEN
            Audit Attempt
            Block User By Entered Credentials Temporarily
            GOTO EndProcess
        ELSE
            Display Error Message on Top Of Verification Form
            GOTO ConnectorD
        ENDIF
    ENDIF

    LABEL EndProcess:
    END