# utl_recovering_after_you_have_painted_yourself_in_a_corner_with_proc_report
Recovering after you have painted yourself in a corner with proc report. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Recovering after you have painted yourself in a corner with proc report

    I would preprocess the data, but if you have substantial processing in proc report,
    You might want ro post process the report output?

     Same results WPS and SAS

    github
    https://github.com/rogerjdeangelis/utl_recovering_after_you_have_painted_yourself_in_a_corner_with_proc_report

    SOAPBOX ON

    This is not a very satisfying solution especially since there is a bug in proc report. Report
    does not honor ODS output. I hope SAS gets back to it's DNA and fixes the
    many isses related to output data structures. We don't need more actors studios and
    starship enterprises.

    Maybe SAS could put proc report across column descriptions in the variable labels?
    Then you could just use the label option in report or print.

    The hard stuff will also help differentiate SAS from WPS, functionality like DOSUBL.

    SOAPBOX OFF

    see
    https://tinyurl.com/yaryy3kp
    https://communities.sas.com/t5/ODS-and-Base-Reporting/Proc-Report-order-by-total-of-across-variable/m-p/446220



    HAVE
    ====

     I added out=havFix and work with the ops 'proc report' output dataset

     proc report data=sashelp.shoes nowd spanrows OUT=HAVFIX;

                                                    Men's          Women's
     Subsidiary            Boot  Men's Casual       Dress          Dress         Total

     Addis Ababa        $29,761       $67,242      $51,541      $108,942      $467,429  * Total out of order
     Algiers            $21,297       $63,206            .       $90,648      $395,600
     Cairo               $4,846      $360,209     $328,474       $14,095      $738,198
     Johannesburg        $8,365             .            .       $42,682      $113,008
     Khartoum           $19,282        $9,244      $19,582       $48,031      $186,592
     Kinshasa           $13,921             .      $17,919       $32,928      $196,816
     Luanda              $6,081       $62,893            .        $8,467      $138,115
     Nairobi            $16,282             .            .       $28,515      $106,830

     Bangkok             $1,996             .       $5,389             .       $16,667
     Seoul              $60,712       $11,754      $20,448       $78,234      $442,409
     Tokyo                    .             .            .             .        $1,155



    WANT   (sort descending by region, tot - region is noprint)
    ==========================================================================
                                                                 Women's
     Subsidiary            Boot  Men's Casual   Men's Dress        Dress         Total   * total is descending
                        $14,979      $112,559       $45,500      $46,789    $2,342,588
     Cairo               $4,846      $360,209        $4,051      $14,095      $738,198
     Addis Ababa        $29,761       $67,242       $76,793     $108,942      $467,429
     Algiers            $21,297       $63,206      $123,743      $90,648      $395,600
     Kinshasa           $13,921             .       $57,691      $32,928      $196,816
     Khartoum           $19,282        $9,244       $18,053      $48,031      $186,592
     Luanda              $6,081       $62,893       $29,582       $8,467      $138,115
     Johannesburg        $8,365             .             .      $42,682      $113,008
     Nairobi            $16,282             .        $8,587      $28,515      $106,830

                        $31,354       $11,754       $59,683      $78,234      $460,231
     Seoul              $60,712       $11,754      $116,333      $78,234      $442,409
     Bangkok             $1,996             .        $3,033            .       $16,667
     Tokyo                    .             .             .            .        $1,155


    PROCESS
    =======

      * may not be needed in the future?;
      * needed because proc report privides no ability to get the ODS column names;
      * build rename string;
      proc sql;
        select cats("_C",put(monotonic()+2,2.),"_ =",quote(lon),'n') into :nams separated by " "
        from
         (select distinct product as lon  from sashelp.shoes(where=(Region in ("Africa" "Asia"))))
      ;quit;

      * You have a dataset so just proc print it. Or put together a minimal 'proc report'.
      * There is not need to list the across variables in the column or define.;
      * Can do alot with the sorted data;

      proc sort data=havFix out=havSrt(rename=(&nams) drop=_break_ region);
        by region descending tot;
      run;quit;

    OUTPUT
    ======

      proc print data=havSrt;
      format _numeric_ Dollar15.2;
      run;quit;


     Subsidiary               Boot     Men's Casual      Men's Dress           Sandal          Slipper       Sport Shoe   Women's Casual    Women's Dress

                        $14,979.38      $112,558.80       $45,500.00       $23,801.13       $42,134.50        $2,768.75      $104,379.00       $46,788.50    $2,342,58
     Cairo               $4,846.00      $360,209.00        $4,051.00       $10,532.00       $13,732.00        $2,259.00      $328,474.00       $14,095.00      $738,19
     Addis Ababa        $29,761.00       $67,242.00       $76,793.00       $62,819.00       $68,641.00        $1,690.00       $51,541.00      $108,942.00      $467,42
     Algiers            $21,297.00       $63,206.00      $123,743.00       $29,198.00       $64,891.00        $2,617.00              .         $90,648.00      $395,60
     Kinshasa           $13,921.00              .         $57,691.00       $16,662.00       $52,807.00        $4,888.00       $17,919.00       $32,928.00      $196,81
     Khartoum           $19,282.00        $9,244.00       $18,053.00       $26,427.00       $43,452.00        $2,521.00       $19,582.00       $48,031.00      $186,59
     Luanda              $6,081.00       $62,893.00       $29,582.00       $11,145.00       $19,146.00          $801.00              .          $8,467.00      $138,11
     Johannesburg        $8,365.00              .                .         $17,337.00       $39,452.00        $5,172.00              .         $42,682.00      $113,00
     Nairobi            $16,282.00              .          $8,587.00       $16,289.00       $34,955.00        $2,202.00              .         $28,515.00      $106,83
                        $31,354.00       $11,754.00       $59,683.00        $4,104.00       $76,016.00        $1,046.00       $12,918.50       $78,234.00      $460,23
     Seoul              $60,712.00       $11,754.00      $116,333.00        $4,978.00      $149,013.00          $937.00       $20,448.00       $78,234.00      $442,40
     Bangkok             $1,996.00              .          $3,033.00        $3,230.00        $3,019.00              .          $5,389.00              .         $16,66
     Tokyo                     .                .                .                .                .          $1,155.00              .                .          $1,15


    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    * SAS;
    proc report data=sashelp.shoes nowd spanrows out=havFix;
        where Region in ("Africa" "Asia");
        column ("Summary of Shoes" (Region Subsidiary) (Sales,Product Sales=tot));
            define Region     / "" group noprint;
            define Subsidiary / "Subsidiary" group;
            define Product    / across;
            define Sales      / analysis mean "";
            define tot        / analysis sum "Total";
        compute before Region / style=[just=l font_style=italic font_weight=bold background=Lightblue foreground=black];
            if Region="Africa" then do;
                text="Africa Region";
            end;
            if Region="Asia" then do;
                text="Asia Region";
            end;
            line text $50.;
        endcomp;
    run;quit;
    proc print data=havFix;
    run;quit;

    * wps;
    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    libname hlp sas7bdat "C:/Program Files/SASHome/SASFoundation/9.4/core/sashelp";
    proc report data=hlp.shoes nowd spanrows out=wrk.havFix;
        where Region in ("Africa" "Asia");
        column ("Summary of Shoes" (Region Subsidiary) (Sales,Product Sales=tot));
            define Region     / "" group noprint;
            define Subsidiary / "Subsidiary" group;
            define Product    / across;
            define Sales      / analysis mean "";
            define tot        / analysis sum "Total";
        compute before Region / style=[just=l font_style=italic font_weight=bold background=Lightblue foreground=black];
            if Region="Africa" then do;
                text="Africa Region";
            end;
            if Region="Asia" then do;
                text="Asia Region";
            end;
            line text $50.;
        endcomp;
    run;quit;
    proc print data=wrk.havfix;
    run;quit;
    ');


    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    *WPS
    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    libname hlp sas7bdat "C:/Program Files/SASHome/SASFoundation/9.4/core/sashelp";
    proc sql;
      select cats("_C",put(monotonic()+2,2.),"_ =",quote(lon),"n") into :nams separated by " "
      from
       (select distinct product as lon  from hlp.shoes(where=(Region in ("Africa" "Asia"))))
    ;quit;

    proc sort data=wrk.havFix out=wrk.havSrt(rename=(&nams) drop=_break_ region);
      by region descending tot;
    run;quit;
    ');


    *SAS;
    proc sql;
      select cats("_C",put(monotonic()+2,2.),"_ =",quote(lon),'n') into :nams separated by " "
      from
       (select distinct product as lon  from sashelp.shoes(where=(Region in ("Africa" "Asia"))))
    ;quit;

    proc sort data=havFix out=havSrt(rename=(&nams) drop=_break_ region);
      by region descending tot;
    run;quit;


