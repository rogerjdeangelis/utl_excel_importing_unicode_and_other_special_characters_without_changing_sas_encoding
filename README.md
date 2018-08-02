# utl_excel_importing_unicode_and_other_special_characters_without_changing_sas_encoding
Excel importing unicode and other special characters without changing sas encoding. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Excel importing unicode and other special characters without changing sas encoding

    If you do not want to restart SAS in unicode mode you can manually 'fix' unicode characters on the excel side

    github
    https://tinyurl.com/yckt56qh
    https://github.com/rogerjdeangelis/utl_excel_importing_unicode_and_other_special_characters_without_changing_sas_encoding
    https://github.com/rogerjdeangelis/utl_import_excel_unicode

    SAS Forum
    https://tinyurl.com/y77r6xbp
    https://communities.sas.com/t5/Base-SAS-Programming/Import-data-from-MS-access-Excel-while-retaining-special/td-p/352237

    Nice more robust solution by
    Vince DelGobbo
    https://communities.sas.com/t5/user/viewprofilepage/user-id/13635



    INPUT
    =====

      d:/xls/utl_excel_importing_unicode_and_other_special_characters_without_changing_sas_encoding.xlsx

          +------------------------+
          |          A             |
          +------------------------+
       1  |                 (SM)   |  ** (SM) is service mark symbol
          | Dunder Crossback       |
          +------------------------+
       2  |                 (SM)   |
          | Dunder Crossback       |
          +------------------------+
       3  |    2                   |  ** superscript 2  (not unicode)
          | Two                    |
          +------------------------+
       4  |    2                   |
          | Two                    |
          +------------------------+
       5  |                        |
          | Two                    |
          +------------------------+

       [CLASS]


    EXAMPLE OUTPUT (SAS dataset WANT)

     WORK.WANT total obs=14

       transformIt

       Dunder Crossback -sm-
       Dunder Crossback -sm-
       Two -sup2-
       Two -sup2-
       Two -sup2-


    PROCESS
    =======

    proc sql dquote=ansi;
     connect to excel
        (Path="d:/xls/utl_excel_importing_unicode_and_other_special_characters_without_changing_sas_encoding.xlsx"
          mixed=yes
          header=no);
        create
            table want as
        select
            *
            from connection to Excel
            (
             Select
                 replace(replace(F1,chr(178),"-sup2-"),chrW(8480),"-sm-") as transformIt
             from
               [sheet1$]
            );
        disconnect from Excel;
    Quit;


    OUTPUT
    ======

    WORK.WANT total obs=14

      TRANSFORMIT

      Dunder Crossback-sm-
      Dunder Crossback-sm-
      Dunder Crossback-sm-
      Dunder Crossback-sm-
      Dunder Crossback-sm-
      Two-sup2-
      Two-sup2-
      Two-sup2-
      Two-sup2-
      Two-sup2-
      Normal
      Normal
      Normal
      Normal


    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    download
    d:/xls/utl_excel_importing_unicode_and_other_special_characters_without_changing_sas_encoding.xlsx

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    see process

    LOG

    1638  proc sql dquote=ansi;
    1639   connect to excel
    1640      (Path="d:/xls/utl_excel_importing_unicode_and_other_special_characters_without_changing_sas_encoding.xlsx"
    1641        mixed=yes
    1642        header=no);
    NOTE: Data source is connected in READ ONLY mode.

    1643      create
    1644          table want as
    1645      select
    1646          *
    1647          from connection to Excel
    1648          (
    1649           Select
    1650               replace(replace(F1,chr(178),"-sup2-"),chrW(8480),"-sm-") as transformIt
    1651           from
    1652             [sheet1$]
    1653          );
    NOTE: Table WORK."WANT" created, with 14 rows and 1 columns.

    1654      disconnect from Excel;
    1655  Quit;
    NOTE: PROCEDURE SQL used (Total process time):
          real time           0.09 seconds
          user cpu time       0.01 seconds

