# TSAna
Simple MPEG transport stream analyser to find discontinuities

    Usage: tsana [options] --filename <filename>
    where options are
    --help                          print *this* usage information and exit
    --version                       print version information and exit
    --dump-first <n>                dump first <n> packets of 188 bytes
    --verbosity <n>                 verbosity level, 0(zero) off, higher value more output, 20(default) is maximum for now
    --max-value <n>                 maximum value for notification of packets in case of an event (default 0)
    --discont-spread <n>            maximum value for spread of packets (default 1000000)
    --discont-capacity <n>          maximum value for entries of packets (default 100)
    --discont-stat {none,dis,big,both}   show statistic of discontinuities of packets (default none)
    --return-value {big,dis,nul}    return value of program in floor(lg(count)), where count is sum of only big(default), all discont or nul(zero).
    ATTENTION: --filename parameter is NOT optional.

Always *\<filename\>* MUST contain an ISO 13818-1 transport stream. The application analyses the outer stream packets to find discontinuities, count them up, and on demand (*--discont-stat dis*) will show some statistic on the distribution of the discontinuities. If there are other errors, the application will show few of them, to show more just use *--max-value \<n\>* with some positive value (the higher the value just more garbarge is on the screen, you are warned). *--dump-first \<n\>* will show up *\<n\>* first packets; I'll need it during develpment. *--discont-spread \<n\>* tells the application where to make the statistical breaks. *--discont-capacity \<n\>* tells the app how many breaks to be able to do. In case you only want the (logarithmic) count of discontinuities via return value you can choose for all(use: *dis*), *big* (only disconts with difference of more than one), or not using the return value (returning negative value in case of failure, otherwise 0).

Output looks like this:

    M:\Video6>tsana.exe --discont-stat dis --filename "Andromeda - Feinde an Bord   Der Geist des Abyss.ts"
    tsana: Gathering transport stream values (MPEG, ISO-13818-1).
    tsana: Andromeda - Feinde an Bord   Der Geist des Abyss.ts
    End. No more data to be read at 3038604144.

    Statistics (spread 1000000)
    Elapsed time  3.8972s
    Payload-Starts  309805
    Packets       16162788 in 3038604144 bytes.
    PID     0        28726, discont       0 (big     0) err  0, scr  0, pri  0, pay       0, adp        0
     00:00:00,000  packets        0 -    28725 :   0/1000 err on   28726 packets,      0 discont w/     0 big
     00:00:00,000  packets        0 -    28725 :   0/1000 err on   28726 packets,      0 big
    PID   128        28726, discont       0 (big     0) err  0, scr  0, pri  0, pay       0, adp        0
     00:00:00,000  packets        0 -    28725 :   0/1000 err on   28726 packets,      0 discont w/     0 big
     00:00:00,000  packets        0 -    28725 :   0/1000 err on   28726 packets,      0 big
    PID   129     15062982, discont    8683 (big   120) err  0, scr  0, pri  0, pay       0, adp   411808
     00:00:00,000  packets        0 -   999999 :   2/1000 err on 1000000 packets,   1132 discont w/     0 big
     00:09:00,120  packets  1000000 -  1999999 :   1/1000 err on 1000000 packets,    271 discont w/     0 big
     00:16:43,560  packets  2000000 -  2999999 :   1/1000 err on 1000000 packets,    503 discont w/     0 big
     00:24:23,640  packets  3000000 -  3999999 :   1/1000 err on 1000000 packets,    802 discont w/     0 big
     00:32:39,480  packets  4000000 -  4999999 :   1/1000 err on 1000000 packets,    104 discont w/     0 big
     00:40:33,240  packets  5000000 -  5999999 :   1/1000 err on 1000000 packets,    456 discont w/     0 big
     00:48:34,320  packets  6000000 -  6999999 :   1/1000 err on 1000000 packets,    362 discont w/    64 big
     00:57:59,640  packets  7000000 -  7999999 :   1/1000 err on 1000000 packets,    388 discont w/     0 big
     01:06:14,760  packets  8000000 -  8999999 :   1/1000 err on 1000000 packets,    247 discont w/     0 big
     01:15:42,000  packets  9000000 -  9999999 :   1/1000 err on 1000000 packets,    679 discont w/     0 big
     01:24:29,520  packets 10000000 - 10999999 :   1/1000 err on 1000000 packets,    640 discont w/    53 big
     01:32:58,200  packets 11000000 - 11999999 :   1/1000 err on 1000000 packets,    590 discont w/     0 big
     01:41:56,280  packets 12000000 - 12999999 :   1/1000 err on 1000000 packets,     77 discont w/     2 big
     01:50:08,040  packets 13000000 - 13999999 :   2/1000 err on 1000000 packets,   1311 discont w/     1 big
     01:59:15,600  packets 14000000 - 14999999 :   2/1000 err on 1000000 packets,   1110 discont w/     0 big
     02:10:52,800  packets 15000000 - 15062981 :   1/1000 err on   62982 packets,     11 discont w/     0 big
    PID   130      1042354, discont      30 (big     8) err  0, scr  0, pri  0, pay       0, adp    54859
     00:00:00,000  packets        0 -   999999 :   1/1000 err on 1000000 packets,     30 discont w/     8 big
     00:00:00,000  packets  1000000 -  1042353 :   0/1000 err on   42354 packets,      0 discont w/     0 big
    Logarithmic sum of discontinues  13.
    Logarithmic sum of big disconts   7.
    END.

Now one can decide if 64 big disconts around 00:48:34 and 53 big disconts around 01:24:29 with a lots of 'little' disconts mostly everywhere are acceptable or not. That's it.

If you are looking for a deep inspection analyser, you are wrong. This tool analyses the continuation counter of the transport stream and shows up, how often the continuation breaks.

Enjoy.
