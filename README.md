# utl-count-grocery-items-and-fraction-dairy-by-customer-using-r-dplyr-language-and-sql-sas-r-python
Count grocery items and fraction dairy by customer using r dplyr language and sql sas r python
    %let pgm=utl-count-grocery-items-and-fraction-dairy-by-customer-using-r-dplyr-language-and-sql-sas-r-python;

    Count grocery items and fraction dairy by customer using r dplyr language and sql sas r python

       SOLUTIONS

          1 r dplyr language
          2 sas sql
          3 r sql
          4 python sql
            Required '1.0' mutiplier 1.0*sum(product='dairy'). Logical type issue? R handled it?

    github
    https://tinyurl.com/4ha67e8z
    https://github.com/rogerjdeangelis/utl-count-grocery-items-and-fraction-dairy-by-customer-using-r-dplyr-language-and-sql-sas-r-python

    stackoverflow R
    https://tinyurl.com/9yhazuy5
    https://stackoverflow.com/questions/79144949/how-can-i-use-dplyr-summarize-to-create-a-count-of-all-items-and-fraction-of-ite

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                   |                                               |                                    */
    /*                                   |                                               |                                    */
    /*           INPUT                   |        PROCESS                                |             OUTPUT                 */
    /*           =====                   |        =======                                |             ======                 */
    /*                                   |                                               |                                    */
    /*                                   |                                               |                                    */
    /*    Does not have to be sorted.    |  SQL (SELF EXPLANATORY - SAME IN R & PYTHON)  |               TOTAL_     DAIRY_    */
    /*    Sorted for explanation only.   |                                               |   CUSTOMER    ORDERS    FRACTION   */
    /*                                   |  SAS SQL                                      |                                    */
    /*                                   |                                               |    Alice         5         0.6     */
    /*   CUSTOMER   PRODUCT              |  select                                       |    Bob           2         0.5     */
    /*                                   |    customer                                   |                                    */
    /*     Alice    dairy                |   ,count(*) as total_orders                   |                                    */
    /*     Alice    dairy                |   ,sum(product='apple')/count(*)              |                                    */
    /*     Alice    dairy                |       as dairy_fraction                       |                                    */
    /*     Alice    meat                 |  from                                         |                                    */
    /*     Alice    poultry              |    sd1.have                                   |                                    */
    /*     Bob      dairy                |  group                                        |                                    */
    /*     Bob      meat                 |    by customer                                |                                    */
    /*                                   |                                               |                                    */
    /*                                   |                                               |                                    */
    /*    ACTUAL DATA                    | CUSTOMER   PRODUCT                            |                                    */
    /*    ===========                    |                                               |                                    */
    /*                                   |   Alice    dairy  total_orders= 5             |                                    */
    /*   CUSTOMER    PRODUCT             |   Alice    dairy  dairy       = 3             |                                    */
    /*                                   |   Alice    dairy  dairy_fraction=3/5= 0.6     |                                    */
    /*      Alice    dairy               |   Alice    meat                               |                                    */
    /*      Alice    dairy               |   Alice    poultry                            |                                    */
    /*      Bob      dairy               |                                               |                                    */
    /*      Alice    meat                |   Bob      dairy                              |                                    */
    /*      Alice    poultry             |   Bob      meat   dairy_fraction=1/2= 0.5     |                                    */
    /*      Bob      meat                |                                               |                                    */
    /*      Alice    dairy               |-----------------------------------------------|                                    */
    /*                                   |                                               |                                    */
    /*                                   | R DPLYR LANGUAGE (|>, .by, ==, summarize)     |                                    */
    /*                                   |                                               |                                    */
    /*                                   | want <- have |>                               |                                    */
    /*                                   |   summarise(                                  |                                    */
    /*                                   |     TOTAL_ORDERS   = n(),                     |                                    */
    /*                                   |     DAIRY_FRACTION = sum(PRODUCT == "dairy")  |                                    */
    /*                                   |      / TOTAL_ORDERS,                          |                                    */
    /*                                   |     .by = CUSTOMER                            |                                    */
    /*                                   |   )                                           |                                    */
    /*                                   |                                               |                                    */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
      input name$ product$;
    cards4;
    Alice apple
    Alice banana
    Alice cherry
    Alice apple
    Bob   apple
    Bob   banana
    Alice apple
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*   CUSTOMER    PRODUCT                                                                                                  */
    /*                                                                                                                        */
    /*    Alice      dairy                                                                                                    */
    /*    Alice      dairy                                                                                                    */
    /*    Bob        dairy                                                                                                    */
    /*    Alice      meat                                                                                                     */
    /*    Alice      poultry                                                                                                  */
    /*    Bob        meat                                                                                                     */
    /*    Alice      dairy                                                                                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _       _             _
    / |  _ __    __| |_ __ | |_   _ _ __ | | __ _ _ __   __ _ _   _  __ _  __ _  ___
    | | | `__|  / _` | `_ \| | | | | `__|| |/ _` | `_ \ / _` | | | |/ _` |/ _` |/ _ \
    | | | |    | (_| | |_) | | |_| | |   | | (_| | | | | (_| | |_| | (_| | (_| |  __/
    |_| |_|     \__,_| .__/|_|\__, |_|   |_|\__,_|_| |_|\__, |\__,_|\__,_|\__, |\___|
                     |_|      |___/                     |___/             |___/
    */
    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(dplyr)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    print(have)
    want <- have |>
      summarise(
        TOTAL_ORDERS   = n(),
        DAIRY_FRACTION = sum(PRODUCT == "dairy") / TOTAL_ORDERS,
        .by = CUSTOMER
      )
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="rwant"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.rwant;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  R                                          SAS                                                                        */
    /*                                                                     TOTAL_     DAIRY_                                  */
    /*    CUSTOMER TOTAL_ORDERS DAIRY_FRACTION     ROWNAMES    CUSTOMER    ORDERS    FRACTION                                 */
    /*                                                                                                                        */
    /*  1 Alice               5            0.6         1        Alice         5         0.6                                   */
    /*  2 Bob                 2            0.5         2        Bob           2         0.5                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                              _
    |___ \   ___  __ _ ___   ___  __ _| |
      __) | / __|/ _` / __| / __|/ _` | |
     / __/  \__ \ (_| \__ \ \__ \ (_| | |
    |_____| |___/\__,_|___/ |___/\__, |_|
                                    |_|
    */

    proc sql;
      create
        table want as
      select
        customer
       ,count(*) as total_orders
       ,sum(product='dairy')/count(*)
           as dairy_fraction
      from
        sd1.have
      group
        by customer
    ;quit;

    proc print data=want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*              TOTAL_     DAIRY_                                                                                         */
    /*  CUSTOMER    ORDERS    FRACTION                                                                                        */
    /*                                                                                                                        */
    /*   Alice         5         0.6                                                                                          */
    /*   Bob           2         0.5                                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____                    _
    |___ /   _ __   ___  __ _| |
      |_ \  | `__| / __|/ _` | |
     ___) | | |    \__ \ (_| | |
    |____/  |_|    |___/\__, |_|
                           |_|
    */

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    print(have)
    want<-sqldf("
      select
        customer
       ,count(*) as total_orders
       ,sum(product='dairy')/count(*)
           as dairy_fraction
      from
        have
      group
        by customer
      ")
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="rrwant"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.rrwant;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  R                                         SAS                                                                         */
    /*                                                                                                                        */
    /*                                                                    TOTAL_     DAIRY_                                   */
    /*    CUSTOMER total_orders dairy_fraction    ROWNAMES    CUSTOMER    ORDERS    FRACTION                                  */
    /*                                                                                                                        */
    /*  1    Alice            5              0        1        Alice         5          0                                     */
    /*  2      Bob            2              0        2        Bob           2          0                                     */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*  _                 _   _                             _
    | || |    _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
    | || |_  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
    |__   _| | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
       |_|   | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
             |_|    |___/                                |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read());
    have,meta = ps.read_sas7bdat('d:/sd1/have.sas7bdat');
    have
    want=pdsql('''
      select
        customer
       ,count(*) as total_orders
       ,1.0*sum(product='dairy')/count(*)
           as dairy_fraction
      from
        have
      group
        by customer
      ''')
    print(want);
    fn_tosas9x(want,outlib='d:/sd1/',outdsn='pywant',timeest=3);
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* R                                           SAS                                                                        */
    /*                                                         TOTAL_     DAIRY_                                              */
    /*    CUSTOMER  total_orders  dairy_fraction   CUSTOMER    ORDERS    FRACTION                                             */
    /*                                                                                                                        */
    /*  0    Alice             5             0.6    Alice         5         0.6                                               */
    /*  1      Bob             2             0.5    Bob           2         0.5                                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
