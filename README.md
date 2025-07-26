# utl-randomly-assign-a-phone-number-from-the-phone-table-to-the-account-table-sas-point
Randomly assign a phone number from the phone table to the account table sas point
    %let pgm=utl-randomly-assign-a-phone-number-from-the-phone-table-to-the-account-table-sas-point;

%stop_submission;

Randomly assign a phone number from the phone table to the account table sas point

fithub
https://tinyurl.com/y82wmrty
https://github.com/rogerjdeangelis/utl-randomly-assign-a-phone-number-from-the-phone-table-to-the-account-table-sas-point

communities.sas
https://tinyurl.com/bdead4sx
https://communities.sas.com/t5/SAS-Programming/How-to-Randomly-Pick-a-Value-from-Other-Table/m-p/970012#M376988

NOTE: There are some faster solutions on sas communities

THREE SOLUTIONS

   1 sas rand(int,beg,end)
   2 sas sql
   3 r sql (like sas sql)

/**************************************************************************************************************************/
/* INPUT                          | PROCESS                                             | OUTPUT                          */
/* =====                          | =======                                             | ======                          */
/* SUPPOSE WE   |                 |  1  SAS RAND("INTEGER",BEG ,END)                    | WANT                            */
/* GENERATE THE |                 |  ===============================                    | STATE    PHONE    ACT           */
/* RANDOM ROWS  |                 |                                                     |                                 */
/*  5 13 18     |                 |  STEPS                                              |  NSW      X1       1            */
/*              |                 |   1 Compute the start and end row number            |  QLD      X12      2            */
/* Table PHONES |  RANDOM         |     for each state                                  |  ROG      X17      3            */
/*              |  ROWS           |   2 Select a random number between the              |                                 */
/* STATE PHONE  |  SELECTED       |     first and last row number by state              |                                 */
/*              |                 |   3 Wse sas point option to select                  |                                 */
/*  NSW   X1    | 1               |     the row number that corresponds                 |                                 */
/*  NSW   X2    |                 |     to the random number.                           |                                 */
/*  NSW   X3    |                 |                                                     |                                 */
/*  NSW   X4    |                 |  data want;                                         |                                 */
/*  NSW   X5    |                 |   call streaminit(11);                              |                                 */
/*  NSW   X6    |                 |   do until (last.state);                            |                                 */
/*  NSW   X7    |                 |      set phones ;                                   |                                 */
/*  QLD   X8    |                 |      by state notsorted;                            |                                 */
/*  QLD   X9    |                 |      ob+1;                                          |                                 */
/*  QLD   X10   |                 |      recs+1;                                        |                                 */
/*  QLD   X11   |                 |   end;                                              |                                 */
/*  QLD   X12   | 12              |   beg=ob-recs+1;                                    |                                 */
/*  QLD   X13   |                 |   pt= rand("integer",beg ,ob);                      |                                 */
/*  QLD   X14   |                 |   recs=0;                                           |                                 */
/*  QLD   X15   |                 |   do until (last.state);                            |                                 */
/*  QLD   X16   |                 |      set accounts;                                  |                                 */
/*  ROG   X17   | 17              |      by state;                                      |                                 */
/*  ROG   X18   |                 |      set phones(keep=phone) point=pt;               |                                 */
/*  ROG   X19   |                 |   end;                                              |                                 */
/*                                |   keep act state phone;                             |                                 */
/*TABLE ACCOUNTS                  | run;quit;                                           |                                 */
/* ACT    STATE                   |                                                     |                                 */
/*                                |---------------------------------------------------------------------------------------*/
/*  1      NSW                    |  2  SAS SQL (LIKE 1)                                | SAS                             */
/*  2      QLD                    |  ===================                                | ACT    STATE    PHONE           */
/*  3      ROG                    |                                                     |                                 */
/*                                |  proc sql;                                          |  1      NSW      X3             */
/* data phones;                   |    create                                           |  2      QLD      X12            */
/* input state $                  |      table want as                                  |  3      ROG      X19            */
/*       phone$;                  |    select                                           |                                 */
/* cards4;                        |      ll.act                                         |                                 */
/* NSW X1                         |     ,l.state                                        |                                 */
/* NSW X2                         |     ,r.phone                                        |                                 */
/* NSW X3                         |    from                                             |                                 */
/* NSW X4                         |      sd1.accounts as ll                             |                                 */
/* NSW X5                         |    left join                                        |                                 */
/* NSW X6                         |      (select                                        |                                 */
/* NSW X7                         |         state                                       |                                 */
/* QLD X8                         |        ,rand("integer",min(rowid),max(rowid))       |                                 */
/* QLD X9                         |          as ranfonrow                               |                                 */
/* QLD X10                        |       from                                          |                                 */
/* QLD X11                        |         sd1.phones                                  |                                 */
/* QLD X12                        |      group                                          |                                 */
/* QLD X13                        |         by state) as l                              |                                 */
/* QLD X14                        |    on                                               |                                 */
/* QLD X15                        |       ll.state = l.state                            |                                 */
/* QLD X16                        |    left join                                        |                                 */
/* ROG X17                        |       sd1.phones as r                               |                                 */
/* ROG X18                        |    on                                               |                                 */
/* ROG X19                        |       ll.state = r.state and                        |                                 */
/* ;;;;                           |       l.ranfonrow=r.rowid                           |                                 */
/* run;quit;                      |                                                     |                                 */
/*                                |  ;quit;                                             |                                 */
/* data accounts;                 |                                                     |                                 */
/* input                          |                                                     |                                 */
/*   act                          |                                                     |                                 */
/*   state $;                     |                                                     |                                 */
/* cards4;                        |                                                     |                                 */
/* 1 NSW                          |                                                     |                                 */
/* 2 QLD                          |                                                     |                                 */
/* 3 ROG                          |                                                     |                                 */
/* ;;;;                           |                                                     |                                 */
/* run;quit;                      |                                                     |                                 */
/*                                |                                                     |                                 */
/*                                ----------------------------------------------------------------------------------------*/
/*                                | USIES BUILT IN ROWID                                |                                 */
/*                                |                                                     |                                 */
/* INPUT WITH ROWID               |  3  R SQL (LIKE SAS SQL)                            | R                               */
/* ================               |  =======================                            |   ACT state PHONE               */
/*                                |                                                     | 1   1   NSW    X2               */
/* options                        |  %utl_rbeginx;                                      | 2   2   QLD   X10               */
/*   validvarname=upcase;         |  parmcards4;                                        | 3   3   ROG   X19               */
/* libname sd1 "d:/sd1";          |  library(haven)                                     |                                 */
/* data sd1.phones;               |  library(sqldf)                                     |                                 */
/* retain rowid 0;                |  source("c:/oto/fn_tosas9x.R")                      | SAS                             */
/* input state $                  |  options(sqldf.dll = "d:/dll/sqlean.dll")           | ACT STATE PHONE                 */
/*  phone$;                       |  phones<-read_sas("d:/sd1/phones.sas7bdat")         |                                 */
/* rowid=_n_;                     |  accounts<-read_sas("d:/sd1/accounts.sas7bdat")     |  1   NSW   X2                   */
/* cards4;                        |  want<-sqldf('                                      |  2   QLD   X10                  */
/* NSW X1                         |  with                                               |  3   ROG   X19                  */
/* NSW X2                         |    ranfon as (                                      |                                 */
/* NSW X3                         |  select                                             |                                 */
/* NSW X4                         |    state                                            |                                 */
/* NSW X5                         |   ,abs(random()) % (max(rowid)                      |                                 */
/* NSW X6                         |      - min(rowid) + 1) + min(rowid) as ranfonrow    |                                 */
/* NSW X7                         |  from                                               |                                 */
/* QLD X8                         |    phones                                           |                                 */
/* QLD X9                         |  group                                              |                                 */
/* QLD X10                        |    by state )                                       |                                 */
/* QLD X11                        |    select                                           |                                 */
/* QLD X12                        |      ll.act                                         |                                 */
/* QLD X13                        |     ,l.state                                        |                                 */
/* QLD X14                        |     ,r.phone                                        |                                 */
/* QLD X15                        |    from                                             |                                 */
/* QLD X16                        |      accounts as ll                                 |                                 */
/* ROG X17                        |    left join                                        |                                 */
/* ROG X18                        |      ranfon as l                                    |                                 */
/* ROG X19                        |    on                                               |                                 */
/* ;;;;                           |       ll.state = l.state                            |                                 */
/* run;quit;                      |    left join                                        |                                 */
/*                                |       phones as r                                   |                                 */
/* data sd1.accounts;             |    on                                               |                                 */
/* input act state $;             |       ll.state = r.state and                        |                                 */
/* cards4;                        |       l.ranfonrow=r.rowid                           |                                 */
/* 1 NSW                          |  ')                                                 |                                 */
/* 2 QLD                          |  want                                               |                                 */
/* 3 ROG                          |  fn_tosas9x(                                        |                                 */
/* ;;;;                           |        inp    = want                                |                                 */
/* run;quit;                      |       ,outlib ="d:/sd1/"                            |                                 */
/*                                |       ,outdsn ="want"                               |                                 */
/*                                |       )                                             |                                 */
/*                                |  ;;;;                                               |                                 */
/*                                |  %utl_rendx;                                        |                                 */
/*                                |                                                     |                                 */
/*                                |  proc print data=sd1.want;                          |                                 */
/*                                |  run;quit;                                          |                                 */
/**************************************************************************************************************************/

/*                   _
(_)_ __  _ __  _   _| |_
| | `_ \| `_ \| | | | __|
| | | | | |_) | |_| | |_
|_|_| |_| .__/ \__,_|\__|
        |_|
*/

data phones;
input state $
      phone$;
cards4;
NSW X1
NSW X2
NSW X3
NSW X4
NSW X5
NSW X6
NSW X7
QLD X8
QLD X9
QLD X10
QLD X11
QLD X12
QLD X13
QLD X14
QLD X15
QLD X16
ROG X17
ROG X18
ROG X19
;;;;
run;quit;

data accounts;
input
  act
  state $;
cards4;
1 NSW
2 QLD
3 ROG
;;;;
run;quit;

/**************************************************************************************************************************/
/* Table PHONES |  TABLE ACCOUNTS                                                                                         */
/*              |   ACT    STATE                                                                                          */
/* STATE PHONE  |                                                                                                         */
/*              |    1      NSW                                                                                           */
/*  NSW   X1    |    2      QLD                                                                                           */
/*  NSW   X2    |    3      ROG                                                                                           */
/*  NSW   X3    |                                                                                                         */
/*  NSW   X4    |                                                                                                         */
/*  NSW   X5    |                                                                                                         */
/*  NSW   X6    |                                                                                                         */
/*  NSW   X7    |                                                                                                         */
/*  QLD   X8    |                                                                                                         */
/*  QLD   X9    |                                                                                                         */
/*  QLD   X10   |                                                                                                         */
/*  QLD   X11   |                                                                                                         */
/*  QLD   X12   |                                                                                                         */
/*  QLD   X13   |                                                                                                         */
/*  QLD   X14   |                                                                                                         */
/*  QLD   X15   |                                                                                                         */
/*  QLD   X16   |                                                                                                         */
/*  ROG   X17   |                                                                                                         */
/*  ROG   X18   |                                                                                                         */
/*  ROG   X19   |                                                                                                         */
/**************************************************************************************************************************/

/*                                      _  ___       _     _                                ___
/ |  ___  __ _ ___  _ __ __ _ _ __   __| |/ (_)_ __ | |_  | |__   ___  __ _   ___ _ __   __| \ \
| | / __|/ _` / __|| `__/ _` | `_ \ / _` | || | `_ \| __| | `_ \ / _ \/ _` | / _ \ `_ \ / _` || |
| | \__ \ (_| \__ \| | | (_| | | | | (_| | || | | | | |_ _| |_) |  __/ (_| ||  __/ | | | (_| || |
|_| |___/\__,_|___/|_|  \__,_|_| |_|\__,_| ||_|_| |_|\__( )_.__/ \___|\__, ( )___|_| |_|\__,_|| |
                                          \_\           |/            |___/|/                /_/
 1  SAS RAND("INTEGER",BEG ,END)
 ===============================

 STEPS
  1 Compute the start and end row number
    for each state
  2 Select a random number between the
    first and last row number by state
  3 Wse sas point option to select
    the row number that corresponds
    to the random number.
*/

 data want;
  call streaminit(11);
  do until (last.state);
     set phones ;
     by state notsorted;
     ob+1;
     recs+1;
  end;
  beg=ob-recs+1;
  pt= rand("integer",beg ,ob);
  recs=0;
  do until (last.state);
     set accounts;
     by state;
     set phones(keep=phone) point=pt;
  end;
  keep act state phone;
run;quit;

/**************************************************************************************************************************/
/* ACT    STATE    PHONE                                                                                                  */
/*  1      NSW      X3                                                                                                    */
/*  2      QLD      X12                                                                                                   */
/*  3      ROG      X19                                                                                                   */
/**************************************************************************************************************************/

/*___                              _
|___ \   ___  __ _ ___   ___  __ _| |
  __) | / __|/ _` / __| / __|/ _` | |
 / __/  \__ \ (_| \__ \ \__ \ (_| | |
|_____| |___/\__,_|___/ |___/\__, |_|
                                |_|
2  SAS SQL (LIKE 1)
===================
*/

proc sql;
  create
    table want as
  select
    ll.act
   ,l.state
   ,r.phone
  from
    sd1.accounts as ll
  left join
    (select
       state
      ,rand("integer",min(rowid),max(rowid))
        as ranfonrow
     from
       sd1.phones
    group
       by state) as l
  on
     ll.state = l.state
  left join
     sd1.phones as r
  on
     ll.state = r.state and
     l.ranfonrow=r.rowid

;quit;
/**************************************************************************************************************************/
/* STATE    PHONE    ACT                                                                                                  */
/*  NSW      X1       1                                                                                                   */
/*  QLD      X12      2                                                                                                   */
/*  ROG      X17      3                                                                                                   */
/**************************************************************************************************************************/

/*                _
 _ __   ___  __ _| |
| `__| / __|/ _` | |
| |    \__ \ (_| | |
|_|    |___/\__, |_|
               |_|

3  R SQL (LIKE SAS SQL)
=======================

USES BUILTIN ROWID
*/

%utl_rbeginx;
parmcards4;
library(haven)
library(sqldf)
source("c:/oto/fn_tosas9x.R")
options(sqldf.dll = "d:/dll/sqlean.dll")
phones<-read_sas("d:/sd1/phones.sas7bdat")
accounts<-read_sas("d:/sd1/accounts.sas7bdat")
want<-sqldf('
with
  ranfon as (
select
  state
 ,abs(random()) % (max(rowid)
    - min(rowid) + 1) + min(rowid) as ranfonrow
from
  phones
group
  by state )
  select
    ll.act
   ,l.state
   ,r.phone
  from
    accounts as ll
  left join
    ranfon as l
  on
     ll.state = l.state
  left join
     phones as r
  on
     ll.state = r.state and
     l.ranfonrow=r.rowid
')
want
fn_tosas9x(
      inp    = want
     ,outlib ="d:/sd1/"
     ,outdsn ="want"
     )
;;;;
%utl_rendx;

proc print data=sd1.want;
run;quit;

/**************************************************************************************************************************/
/*  > want                   |                                                                                            */
/*    ACT state PHONE   ACT  |  STATE    PHONE                                                                            */
/*                           |                                                                                            */
/*  1   1   NSW    X7    1   |   NSW      X7                                                                              */
/*  2   2   QLD   X12    2   |   QLD      X12                                                                             */
/*  3   3   ROG   X19    3   |   ROG      X19                                                                             */
/**************************************************************************************************************************/

/*              _
  ___ _ __   __| |
 / _ \ `_ \ / _` |
|  __/ | | | (_| |
 \___|_| |_|\__,_|

*/
