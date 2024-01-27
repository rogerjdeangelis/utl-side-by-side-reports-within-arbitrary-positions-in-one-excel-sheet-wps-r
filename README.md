# utl-side-by-side-reports-within-arbitrary-positions-in-one-excel-sheet-wps-r
Side by side reports within arbitrary positions in one excel sheet wps r
    %let pgm=utl-side-by-side-reports-within-arbitrary-positions-in-one-excel-sheet-wps-r;

    Side by side reports within arbitrary positions in one excel sheet wps r

    github
    http://tinyurl.com/t2zfsrac
    https://github.com/rogerjdeangelis/utl-side-by-side-reports-within-arbitrary-positions-in-one-excel-sheet-wps-r

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                       |                                |                                                               */
    /*        INPUT          |       PROCESS                  |                        OUTPUT                                 */
    /*                       |                                |                                                               */
    /* SD1.HAVE total obs=10 |  males  <-have[have$SEX=="M",];|  d:/xls/sex_fm.xlsx (reports positioned at B3 and G3          */
    /*                       |  females<-have[have$SEX=="F",];|                                                               */
    /* Obs    NAME       SEX |                                |   +---------------------+-----------------------------------+ */
    /*                       |  wb <- loadWorkbook(           |   |  A  |  B    |  C    |  D  |  E  |  F    |  G    |  H    | */
    /*   1    Alfred      M  |      "d:/xls/sex_mf.xlsx"      |   +---------------------+-----------------------------------+ */
    /*   2    Alice       F  |      ,create = TRUE);          |  1|     |       |       |     |     |       |       |       | */
    /*   3    Barbara     F  |                                |   |-----+-------+-------|-----+-----+-------+-------+-------| */
    /*   4    Carol       F  |  writeWorksheet                |  2|     |       |       |     |     |       |       |       | */
    /*   5    Henry       M  |         (wb                    |   |-----+-------+-------+-----+-----+-------+-------+-------+ */
    /*   6    James       M  |         ,males                 |  3|     |NAME   |SEX    |     |     |       |NAME   |SEX    | */
    /*   7    Jane        F  |         ,sheet = "sex"         |   |-----+-------+-------|-----+-----+-------+-------+-------| */
    /*   8    Janet       F  |         ,startRow = 3          |  4|     |Alfred |M      |     |     |       |Alice  |F      | */
    /*   9    Jeffrey     M  |         ,startCol = 2          |   |-----+-------+-------+-----+-----+-------+-------+-------+ */
    /*  10    John        M  |         ,header = TRUE);       |  5|     |Alex   |M      |     |     |       |Barbara|F      | */
    /*                       |                                |   |-----+-------+-------+-----+-----+-------+-------+-------+ */
    /*                       |   writeWorksheet(              |  6|     |Bob    |M      |     |     |       |Carol  |F      | */
    /*                       |         wb                     |   |-----+-------+-------+-----+-----+-------+-------+-------+ */
    /*                       |         ,females               |  7|     |Chris  |M      |     |     |       |Jane   |F      | */
    /*                       |         ,sheet = "sex"         |   |-----+-------+-------+-----+-----+-------+-------+-------+ */
    /*                       |         ,startRow = 3          |  8|     |Henry  |M      |     |     |       |       |       | */
    /*                       |         ,startCol = 2          |   ----------------------------------------------------------- */
    /*                       |         ,header = TRUE);       |                                                               */
    /*                       |                                |  [SEX]                                                        */
    /*                       |                                |                                                               */
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
      set sashelp.class(keep=name sex obs=10);
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* SD1.HAVE total obs=10                                                                                                  */
    /*                                                                                                                        */
    /* Obs    NAME       SEX                                                                                                  */
    /*                                                                                                                        */
    /*   1    Alfred      M                                                                                                   */
    /*   2    Alice       F                                                                                                   */
    /*   3    Barbara     F                                                                                                   */
    /*   4    Carol       F                                                                                                   */
    /*   5    Henry       M                                                                                                   */
    /*   6    James       M                                                                                                   */
    /*   7    Jane        F                                                                                                   */
    /*   8    Janet       F                                                                                                   */
    /*   9    Jeffrey     M                                                                                                   */
    /*  10    John        M                                                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    %utlfkil(d:/xls/sex_mf.xlsx);

    %utl_submit_r64x('
    library(haven);
    library(XLConnect);
    have<-read_sas("d:/sd1/have.sas7bdat");
    have;
    males<-have[have$SEX=="M",];
    females<-have[have$SEX=="F",];
    wb <- loadWorkbook("d:/xls/sex_mf.xlsx", create = TRUE);
    createSheet(wb, name = "sex");
    writeWorksheet(wb, males, sheet = "sex", startRow = 3,startCol = 2, header = TRUE);
    writeWorksheet(wb, females, sheet = "sex", startRow = 3,startCol = 7, header = TRUE);
    saveWorkbook(wb);
    ');

     /**************************************************************************************************************************/
     /*                                                                                                                        */
     /*  d:/xls/sex_fm.xlsx (reports positioned at B3 and G3)                                                                  */
     /*                                                                                                                        */
     /*   +---------------------+-----------------------------------+                                                          */
     /*   |  A  |  B    |  C    |  D  |  E  |  F    |  G    |  H    |                                                          */
     /*   +---------------------+-----------------------------------+                                                          */
     /*  1|     |       |       |     |     |       |       |       |                                                          */
     /*   |-----+-------+-------|-----+-----+-------+-------+-------|                                                          */
     /*  2|     |       |       |     |     |       |       |       |                                                          */
     /*   |-----+-------+-------+-----+-----+-------+-------+-------+                                                          */
     /*  3|     |NAME   |SEX    |     |     |       |NAME   |SEX    |                                                          */
     /*   |-----+-------+-------|-----+-----+-------+-------+-------|                                                          */
     /*  4|     |Alfred |M      |     |     |       |Alice  |F      |                                                          */
     /*   |-----+-------+-------+-----+-----+-------+-------+-------+                                                          */
     /*  5|     |Alex   |M      |     |     |       |Barbara|F      |                                                          */
     /*   |-----+-------+-------+-----+-----+-------+-------+-------+                                                          */
     /*  6|     |Bob    |M      |     |     |       |Carol  |F      |                                                          */
     /*   |-----+-------+-------+-----+-----+-------+-------+-------+                                                          */
     /*  7|     |Chris  |M      |     |     |       |Jane   |F      |                                                          */
     /*   |-----+-------+-------+-----+-----+-------+-------+-------+                                                          */
     /*  8|     |Henry  |M      |     |     |       |       |       |                                                          */
     /*   -----------------------------------------------------------                                                          */
     /*                                                                                                                        */
     /* [SEX]                                                                                                                  */
     /*                                                                                                                        */
     /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
