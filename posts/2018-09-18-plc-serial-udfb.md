# A design pattern for serial comm UDFBs

The purpose of this post is to propose a [Structured Text](https://en.wikipedia.org/wiki/Structured_text) (ST) [state-machine](https://en.wikipedia.org/wiki/State_pattern) design pattern for User Defined Function Blocks (UDFBs) for serial communication. The samples in this post are intended for [Connected Components Workbench](https://www.rockwellautomation.com/global/detail.page?docid=7c028f3b448fcef185a20f4e05c5fd06) (CCW).
At the time of writing, no articles on design patterns for such a use-case were easily available. Ladder language is still the most popular framework for programming PLCs, but its graphical representation is inconvenient for code sharing. Code sharing is easier and resuable when it is plain text, and indeed user-generated structured-text code hubs can be found.

## Why use a UDFB instead of a Main program?
Structured Text has some object-oriented characteristics and the UDFB is one of them. By using a UDFB, it is possible to create multiple instances of the same Program Organization Unit (POU). In industrial automation applications it is likely that there will be several identical devices connected to the same [PLC](https://en.wikipedia.org/wiki/Programmable_logic_controller). Therefore, declaring several instances of the same UDFB is preferable to duplicating code into several Main programs, for obvious reasons.

## General structure of a reentrant UDFB
Assuming the serial comm UDFB is invoked from a Main program and expected to continuously poll for data, the UDFB needs to address enabling, disabling, and continuous operation.

```structured-text
FBENO := FBEN;
R_TRIG_FBEN(FBEN);
F_TRIG_FBEN(FBEN);

// Construction
IF __SYSVA_FIRST_SCAN OR R_TRIG_FBEN.Q THEN
    Q := FALSE;
    // ...
END_IF;


// Destruction
IF F_TRIG_FBEN.Q THEN
    // If Q is reset here then under some conditions the caller may 
    // miss the window when Q is TRUE.
    // Here, the UDFB is designed to run continuously so setting Q to 
    // FALSE is convenient.
    Q := FALSE;
    // ...
END_IF;


IF FBEN THEN
    // Main program logic goes here
    
    CASE State OF
    
        0:  // Init
        
        10: // Pre-processing (PACK_QUERY)
        
        20: // Perform IO (SEND_AND_RECV)
        
        30: // Post-processing (UNPACK_RESPONSE)
        
        40: // Loop delay (TON_Loop_Delay)
    
    ELSE
    
    END_CASE; // State
    
ELSE // FBEN is FALSE
    // OFF logic (if any) goes here

END_IF;


// UDFB calls go here
PACK_QUERY_1(FBEN:=en_pack, ...);
SEND_AND_RECV_1(FBEN:=en_txrx, ...);
UNPACK_RESPONSE_1(FBEN:=en_unpack, ...);
TON_Loop_Delay(IN:=en_delay, ...);
````

## Case structure (the state machine)
A case structure is probably the most convenient way to track progress from scan to scan.

Best practices:
1. Use Defined Words (constants) as case labels. Avoid explicit numerals when possible.
1. Repeat all UDFB enable flags at the beginning of each case.
1. Limit the modification of variables internal to the case structure to as few cases as possible for maximal code maintainability.

<table>
  <tr>
    <th>Word</th>
    <th>Equivalent</th>
    <th>Comment</th>
  </tr>
  <tr>
    <td>STATE_Init</td>
    <td>0</td>
    <td>Initialize enable flags to false, initialize indices to zero</td>
  </tr>
  <tr>
    <td>STATE_Query_cycling</td>
    <td>10</td>
    <td>Increment query index, initialize query (if generic)</td>
  </tr>
  <tr>
    <td>STATE_Pre_processing</td>
    <td>20</td>
    <td>Finalize and pack query (if needed)</td>
  </tr>
  <tr>
    <td>STATE_Perform_IO</td>
    <td>30</td>
    <td>Wait for communication to complete (subject to timeout), verify integrity</td>
  </tr>
  <tr>
    <td>STATE_Post_processing</td>
    <td>40</td>
    <td>Unpack query (if needed)</td>
  </tr>
  <tr>
    <td>STATE_Loop_Delay</td>
    <td>50</td>
    <td>Wait before sending the next query (if needed)</td>
  </tr>
</table>

````structured-text
CASE State OF

    STATE_Init:
    
        en_pack := FALSE;
        en_txrx := FALSE;
        en_unpack := FALSE;
        en_delay := FALSE;
        
        // Initialization code goes here
        query_num := 0;
        query := '';
        
        State := STATE_Query_cycling;
        
    
    
    // Use this case if more than one query is neeeded
    STATE_Query_cycling:
        
        en_pack := FALSE;
        en_txrx := FALSE;
        en_unpack := FALSE;
        en_delay := FALSE;
        
        // Preliminary query building code goes here
        CASE query_num OF
            ...
            query := ...;
            ...
        ELSE
            query_num := 0;
            query := '';
            
        END_CASE;
        
        IF query = '' THEN
            State := STATE_Loop_Delay;
        ELSE
            State := STATE_Pre_processing;
        END_IF;
                
    
    
    STATE_Pre_processing:
        
        en_pack := TRUE;
        en_txrx := FALSE;
        en_unpack := FALSE;
        en_delay := FALSE;
        
        // Final query building code goes here
        
        IF PACK_QUERY_1.Q THEN
            State := STATE_Perform_IO;
        END_IF;
        
    
    
    STATE_Perform_IO:
        
        en_pack := FALSE;
        en_txrx := TRUE;
        en_unpack := FALSE;
        en_delay := FALSE;
        
        IF SEND_AND_RECV_1.Q THEN
            // Integrity check code goes here
            IF SERIAL_SEND_AND_RECV_STR_1.OK THEN
                State := STATE_Post_processing;
            ELSE
                State := STATE_Loop_Delay;
            END_IF;
        END_IF;
        
    
    
    STATE_Post_processing:
        
        en_pack := FALSE;
        en_txrx := FALSE;
        en_unpack := TRUE;
        en_delay := FALSE;
        
        IF UNPACK_RESPONSE_1.Q THEN
            // Response processing code goes here
            State := STATE_Loop_Delay;
        END_IF;
        
    
    
    STATE_Loop_Delay:
        
        en_pack := FALSE;
        en_txrx := FALSE;
        en_unpack := FALSE;
        en_delay := TRUE;
        
        IF TON_Loop_Delay.Q THEN
            
            query_num := query_num + 1;
            IF query_num > query_nums THEN
                State := STATE_Init;
            ELSE
                State := STATE_Query_cycling;
            END_IF;
            
        END_IF;
        
    
    ELSE
    
        en_pack := FALSE;
        en_txrx := FALSE;
        en_unpack := FALSE;
        en_delay := FALSE;
        
        State := STATE_INIT;
        

END_CASE; // State
````

