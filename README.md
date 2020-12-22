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

If you are looking for a deep inspection analyser, you are wrong. This tool analyses the continuation counter of the transport stream and shows up, how often the continuation breaks.

Enjoy.
