# utl-google-trends-is-the-seach-word-increasing-in-popularity
Google trends is the seach word increasing in popularity
    Google trends is the seach word increasing in popularity

    github
    https://tinyurl.com/yye8akft
    https://github.com/rogerjdeangelis/utl-google-trends-is-the-seach-word-increasing-in-popularity

    SAS Forum ( I could not get the link to work)
    https://tinyurl.com/yxjdazcp
    https://communities.sas.com/t5/SAS-Programming/data-extraction-from-google-trend/m-p/535550

    Source
    https://www.displayr.com/extracting-google-trends-data-in-r/


    BACKGROUND

    Google Trends shows the changes in the popularity of search terms over a given time (i.e., number of hits over time).
    It can be used to find search terms with growing or decreasing popularity or to review periodic variations from
    the past such as seasonality. Google Trends search data can be added to other analyses,
    manipulated and explored in more detail in R

    INPUT
    =====

      Search Word "DeAngelis"

    EXAMPLE OUTPUT
    --------------


     Relative Month to Month Trend of Search Word 'DeAngelis'
     The maximum is 100
     Feb is not over yet?

     DEANGELIS (Maximum is 100)
                                     27
     27 +----------------------------*
        |                            |
        |                            |
        |                            |
     25 +                             > Change from 23 to 27 or 4/23 = 17% Increase
        |                            |
        |                            |
        |  23           23           |
     23 +--*------------*------------+
        |
        |
        |                                    21
     21 +                                     *
        ---+------------+------------+--------+--
         NOV18        DEC18        JAN19    FEB19
                                            Partial Month?
                            DATE

    SOLUTION
    ========

    %utl_submit_r64('
    library(gtrendsR);
    library(reshape2);
    library(SASxport);
    google.trends <- gtrends(c("DeAngelis"), gprop = "web", time = "all")[[1]];
    google.trends <- dcast(google.trends, date ~ keyword + geo, value.var = "hits");
    want<-google.trends;
    want$date<-google.trends$date;
    write.xport(want,file="d:/xpt/want.xpt");
    ');

    libname xpt xport "d:/xpt/want.xpt";
    data want;
      format date monyy.;
      set xpt.want;
      datec=put(date,monyy.);
    run;quit;
    libname xpt clear;

    /*
    WANT total obs=182

    Obs      DATE       WPS_WORL

      1    01JAN2004       68
      2    01FEB2004       83
      3    01MAR2004       88
      4    01APR2004       92
      5    01MAY2004       77

    ....
    */

    options ls=64 ps=24;
    proc plot data=want(where=(date>'01OCT2018'd));
    title "Relative Month to Month Trend of Search Word WPS";
    plot deangeli*datec / haxis='NOV18' 'DEC18' 'JAN19' 'FEB19'
    vref=23 27;
    run;quit;

