# TSAna
Simple MPEG transport stream analyser to find discontinuities

    Usage: tsana [options] --filename <filename>
    where options are
    --help                          print *this* usage information and exit
    --version                       print version information and exit
    --dump-first <n>                dump first <n> packets of 188 bytes
    --verbosity <n>                 verbosity level, 0(zero) off, higher value more output, default 20, maximum 25
    --max-value <n>                 maximum value for notification of packets in case of an event (default 0)
    --discont-spread <n>            maximum value for spread of time in seconds (default 60)
    --discont-capacity <n>          maximum value for entries of packets (default 10000)
    --discont-stat {none,dis}       show statistic of discontinuities of packets (default none)
    --discont-reverse               count discontinuities even if in reverse (resend packets?)
    --return-value {big,dis,nul}    return value of program in floor(lg(count)), where count is sum of only big(default), all discont or nul(zero).
    ATTENTION: --filename parameter is NOT optional.

Always *\<filename\>* MUST contain an ISO 13818-1 transport stream. The application analyses the outer stream packets to find discontinuities, count them up, and on demand (*--discont-stat dis*) will show some statistic on the distribution of the discontinuities. If there are other errors, the application will show few of them, to show more just use *--max-value \<n\>* with some positive value (the higher the value just more garbarge is on the screen, you are warned). *--dump-first \<n\>* will show up *\<n\>* first packets; I'll need it during develpment. *--discont-spread \<n\>* tells the application where to make the statistical breaks. *--discont-capacity \<n\>* tells the app how many breaks to be able to do. In case you only want the (logarithmic) count of discontinuities via return value you can choose for all(use: *dis*), *big* (only disconts with difference of more than one), or not using the return value (returning negative value in case of failure, otherwise 0).

Output looks like this:

    $ tsana.exe --discont-stat dis --discont-spread 10 --filename "Das geheime Fenster.ts"
    tsana: Gathering transport stream values (MPEG, ISO-13818-1).
    tsana: Das geheime Fenster_2021-01-16_22-20-04_zdf_neo HD (deu).ts
    End. No more data to be read at 2659717592.

    Statistics (spread 10s)
    Elapsed time   96.9542s
    PMT stream PID     2300
    Elementary PIDs
      2310 ITU - T Rec.H.265 and ISO / IEC 23008 - 2 (Ultra HD video) in a packetized stream (24h)
      2320 ITU - T Rec.H.222 and ISO / IEC 13818 - 1 (MPEG - 2 packetized data) privately defined(i.e., DVB subtitles / VBI and AC - 3) (06h)
      2321 ITU - T Rec.H.222 and ISO / IEC 13818 - 1 (MPEG - 2 packetized data) privately defined(i.e., DVB subtitles / VBI and AC - 3) (06h)
      2322 ITU - T Rec.H.222 and ISO / IEC 13818 - 1 (MPEG - 2 packetized data) privately defined(i.e., DVB subtitles / VBI and AC - 3) (06h)
    PCR stream PID     2310
    Payload-Starts   534348
    Packets        14147434 in 2659717592 bytes.
    Length         01:49:55,320
    PID     0         16434, discont       0 (big     0) err  0, scr  0, pri  0, pay   16434, adp        0
    PID  2300         64817, discont       0 (big     0) err  0, scr  0, pri  0, pay       0, adp        0
    PID  2310      11759530, discont      24 (big    23) err  0, scr  0, pri  0, pay  329525, adp   589950
     00:33:20,000  packets  3747005 -  3764489 :   1/1000 err on   17485 packets,      7 discont w/     7 big
     00:35:20,000  packets  3987395 -  4007741 :   1/1000 err on   20347 packets,     12 discont w/    12 big
     00:35:32,600  packets  4007742 -  4022295 :   1/1000 err on   14554 packets,      5 discont w/     4 big
    PID  2320       1153325, discont       8 (big     5) err  0, scr  0, pri  0, pay   41190, adp    41190
     00:33:20,000  packets   350002 -   351764 :   1/1000 err on    1763 packets,      1 discont w/     0 big
     00:35:20,000  packets   371001 -   372510 :   3/1000 err on    1510 packets,      4 discont w/     3 big
     00:35:32,640  packets   372511 -   373666 :   3/1000 err on    1156 packets,      3 discont w/     2 big
    PID  2321        576664, discont       5 (big     3) err  0, scr  0, pri  0, pay   41191, adp    41191
     00:33:20,000  packets   175002 -   175882 :   2/1000 err on     881 packets,      1 discont w/     0 big
     00:35:20,000  packets   185501 -   186242 :   5/1000 err on     742 packets,      3 discont w/     3 big
     00:35:32,480  packets   186243 -   186835 :   2/1000 err on     593 packets,      1 discont w/     0 big
    PID  2322        576664, discont       5 (big     3) err  0, scr  0, pri  0, pay   41191, adp    41191
     00:33:20,000  packets   175002 -   175881 :   2/1000 err on     880 packets,      1 discont w/     1 big
     00:35:20,000  packets   185500 -   186242 :   5/1000 err on     743 packets,      3 discont w/     2 big
     00:35:32,480  packets   186243 -   186835 :   2/1000 err on     593 packets,      1 discont w/     0 big
    Logarithmic sum of discontinues   5.
    Logarithmic sum of big disconts   5.
    END.

Now, decision is easier if for example 23 big disconts from 00:33:20 till 00:36:32,6 (spread 60s) are acceptable or not. That's it.

If you are looking for a deep inspection analyser for elementary streams you are wrong. This tool analyses the continuation counter of an ISO-13818-1 transport stream only and shows how often the continuation counter breaks.

Enjoy.
