# datetidy
Bash script to select (and do something, eg delete) files that match date criteria

## examples

###       Give all except the most recent 2
datetidy --policy "count=-2" *

###       Give most recent 3 that are at least one month old
datetidy --reverse --policy "ed:now - 1 month;count=3" *

###      Give oldest 3 that are at most than one month old
datetidy --policy "sd:now - 1 month;count=3" *

###       Give all files created on Monday
datetidy --policy "doy:Mon" *

###       Give all files created on 2nd of month
datetidy --policy "dom:2" *

###       Give all files not created in 2018
datetidy --nomatch --policy "year=2018" *

###       Give most recent 4 files up to end of 2018
datetidy --reverse --policy "ed=1Jan2019;count=4"

###       Give all files except most recent 1 from each last 1 day, 2,4,8,16 days
datetidy --reverse --policy "group=day;count=-5;mode=binary"

###       Give all files except in last week and 
###          most recent 1 from each last 1 day, 2,4,8,16,32,64,128,256,512 days
datetidy --reverse --policy "ed=now - 7 days;group=day;count=-10;mode=binary"

## Usage

~~~
Usage: datetidy [options] filespec
        Options:

        --help          show this help screen
        --examples      show some examples
        --verbose       control message level (repeat for more information)
        --quiet         control message level (repeat for less information)
        --[no]match     return names of matching files (default: --match)
        --[no]reversed  sort newest first (no=last) (default: --noreversed)
        --[no]all       show all (with tag: selected, not selected, skipped)
                           will not also process files, but will do -exec cmd
        --policy ps     specify the policy string
                           enclose in quotes if contains spaces or punctuation
                           ps is a semi-colon delimited list of the following 
                            sd:startDate  the start date (default = day0)
                            ed:endDate    the end date (default = now)
                                Note: match when startDate <= fileDate < endDate
                                Note: give date as dd-mm-yyyy [hh[:mm[:ss]]]
                           regular expressions compare:
                            dow:Mon|Tue|...  day of week
                            year:n           year (4-digit)
                            month:n          month of year
                            dom:n            day of month
                            hour:n           hour (24hr clock)
                            minute:n         match minute
                           how many to keep is set by:
                            count:n          how many to keep (default=0 ie all)
                                                n<0 ie all-n
                            group:y|m|w|d|h  group count by 
                                   year|month|week|day|hour
                            mode:none|binary|linear counting scheme (default:none)
                                   none   - the match 'count' in each group
                                when combined with group, match 'count' in each
                                set of recent group intervals (default=day)
                                   binary - 1,2,4,8,...
                                   linear - 1,2,3,4,...
                        if no policy is specified, a default policy is used:
                          sd            ie include everything
        --[no]test      show keep or do commands, don't actually do them
        --do cmd        run command on each of the files selected
                                if cmd contains {} the filename is substituted
                                if not, the filename will be appended

        short options use the initial letter of long options:
                -hdvqpmrae      use (eg) -m- as short version of --nomatch
~~~
