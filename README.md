# utl-when-a-sentence-is-spread-over-many-lines-combine-lines-into-one-record-sas-infile-python
    %let pgm=utl-when-a-sentence-is-spread-over-many-lines-combine-lines-into-one-record-sas-infile-python;

    When a sentence is spread over many lines combine lines into one record sas infile python

    Best in classic 1980's editor?

       SOLUTIONS
           1 SAS (output is a dataframe)
           2 Python (very readable)
             https://stackoverflow.com/users/1883316/tim-roberts

    github
    https://tinyurl.com/3tcvxrjw
    https://github.com/rogerjdeangelis/utl-when-a-sentence-is-spread-over-many-lines-combine-lines-into-one-record-sas-infile-python

    stackoverflow
    https://tinyurl.com/37hd5wvx
    https://stackoverflow.com/questions/78864083/string-manipulation-using-readlines-and-organizing-results-based-off-next-line

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /*****************************************************************************************************************************/
    /*                                  |                                             |                                          */
    /*         INPUT                    |  SAS PROCESS                                |            OUTPUT                        */
    /*         =====                    |  ===========                                |            ======                        */
    /*                                  |                                             |                                          */
    /* Sentences spread across lines    |  data want;                                 | set word1 word2 word3 word4 word5 word6  */
    /*                                  |                                             | set word1 word2 word3 word4 word5 word6  */
    /* set word1 word2                  |    length last line $255;                   | set word1 word2 word3 word4 word5 word6  */
    /* word3 word4                      |    retain line;                             | set word1 word2 word3 word4 word5 word6  */
    /* word5 word6                      |    format word $24.;                        |                                          */
    /*                                  |                                             |                                          */
    /* set word1 word2 word3 word4      |    infile cards4 eof=dne;                   |                                          */
    /* word5                            |    input word @@;                           |                                          */
    /* word6                            |    /*----                            ----*/ |                                          */
    /*                                  |    /*----  skip over the first word  ----*/ |                                          */
    /* set word1 word2 word3 word4 word5|    /*----                            ----*/ |                                          */
    /* word6                            |    if _n_>1;                                |                                          */
    /*                                  |                                             |                                          */
    /* set                              |    line=catx(' ',line,word);                |                                          */
    /* word1                            |    last=catx(' ','set',compress(line,'set'))|                                          */
    /* word2                            |                                             |                                          */
    /* word3                            |    if word =: 'set' then                    |                                          */
    /* word4 word5                      |      do;                                    |                                          */
    /* word6                            |        output;                              |                                          */
    /*                                  |        line='';                             |                                          */
    /*                                  |      end;                                   |                                          */
    /*                                  |    return;                                  |                                          */
    /*                                  |    dne:                                     |                                          */
    /*                                  |      last=catx('','set',line);              |                                          */
    /*                                  |      output;                                |                                          */
    /*                                  |    keep last;                               |                                          */
    /*                                  |  cards4;                                    |                                          */
    /*                                  |  set word1 word2                            |                                          */
    /*                                  |  word3 word4                                |                                          */
    /*                                  |  word5 word6                                |                                          */
    /*                                  |  set word1 word2 word3 word4                |                                          */
    /*                                  |  word5                                      |                                          */
    /*                                  |  word6                                      |                                          */
    /*                                  |  set word1 word2 word3 word4 word5          |                                          */
    /*                                  |  word6                                      |                                          */
    /*                                  |  set                                        |                                          */
    /*                                  |  word1                                      |                                          */
    /*                                  |  word2                                      |                                          */
    /*                                  |  word3                                      |                                          */
    /*                                  |  word4 word5                                |                                          */
    /*                                  |  word6                                      |                                          */
    /*                                  |  ;;;;                                       |                                          */
    /*                                  |  run;quit;                                  |                                          */
    /*                                  |                                             |                                          */
    /*                                  | ON PROCESS                                  |                                          */
    /*                                  |                                             |                                          */
    /*                                  |  %utl_pybeginx;                             |   SAME OUTPUT                            */
    /*                                  |  parmcards4;                                |                                          */
    /*                                  |  data = """\                                |                                          */
    /*                                  |  set word1 word2                            |                                          */
    /*                                  |  word3 word4                                |                                          */
    /*                                  |  word5 word6                                |                                          */
    /*                                  |  set word1 word2 word3 word4                |                                          */
    /*                                  |  word5                                      |                                          */
    /*                                  |  word6                                      |                                          */
    /*                                  |  set word1 word2 word3 word4 word5          |                                          */
    /*                                  |  word6                                      |                                          */
    /*                                  |  set                                        |                                          */
    /*                                  |  word1                                      |                                          */
    /*                                  |  word2                                      |                                          */
    /*                                  |  word3                                      |                                          */
    /*                                  |  word4 word5                                |                                          */
    /*                                  |  word6"""                                   |                                          */
    /*                                  |                                             |                                          */
    /*                                  |                                             |                                          */
    /*                                  |  gather = ''                                |                                          */
    /*                                  |  for line in data.splitlines():             |                                          */
    /*                                  |      if line.startswith('set'):             |                                          */
    /*                                  |          if gather:                         |                                          */
    /*                                  |              print(gather)                  |                                          */
    /*                                  |          gather = line                      |                                          */
    /*                                  |      else:                                  |                                          */
    /*                                  |          gather += ' '+line                 |                                          */
    /*                                  |  if gather:                                 |                                          */
    /*                                  |      print(gather)                          |                                          */
    /*                                  |  ;;;;                                       |                                          */
    /*                                  |  %utl_pyendx;                               |                                          */
    /*                                  |                                             |                                          */
    /*****************************************************************************************************************************/

    /*
    / |  ___  __ _ ___
    | | / __|/ _` / __|
    | | \__ \ (_| \__ \
    |_| |___/\__,_|___/

    */

    data want;

      length last line $255;
      retain line;
      format word $24.;

      infile cards4 eof=dne;
      input word @@;
      /*----                            ----*/
      /*----  skip over the first word  ----*/
      /*----                            ----*/
      if _n_>1;

      line=catx(' ',line,word);
      last=catx(' ','set',compress(line,'set'));

      if word =: 'set' then
        do;
          output;
          line='';
        end;
      return;
      dne:
        last=catx('','set',line);
        output;
      keep last;
    cards4;
    set word1 word2
    word3 word4
    word5 word6
    set word1 word2 word3 word4
    word5
    word6
    set word1 word2 word3 word4 word5
    word6
    set
    word1
    word2
    word3
    word4 word5
    word6
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* WORK.WANT total obs=4                                                                                                  */
    /*                                                                                                                        */
    /*                   LAST                                                                                                 */
    /*                                                                                                                        */
    /*  set word1 word2 word3 word4 word5 word6                                                                               */
    /*  set word1 word2 word3 word4 word5 word6                                                                               */
    /*  set word1 word2 word3 word4 word5 word6                                                                               */
    /*  set word1 word2 word3 word4 word5 word6                                                                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                _   _
    |___ \   _ __  _   _| |_| |__   ___  _ __
      __) | | `_ \| | | | __| `_ \ / _ \| `_ \
     / __/  | |_) | |_| | |_| | | | (_) | | | |
    |_____| | .__/ \__, |\__|_| |_|\___/|_| |_|
            |_|    |___/
    */

    %utl_pybeginx;
    parmcards4;
    data = """\
    set word1 word2
    word3 word4
    word5 word6
    set word1 word2 word3 word4
    word5
    word6
    set word1 word2 word3 word4 word5
    word6
    set
    word1
    word2
    word3
    word4 word5
    word6"""

    gather = ''
    for line in data.splitlines():
        if line.startswith('set'):
            if gather:
                print(gather)
            gather = line
        else:
            gather += ' '+line
    if gather:
        print(gather)
    ;;;;
    %utl_pyendx;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* WORK.WANT total obs=4                                                                                                  */
    /*                                                                                                                        */
    /*                   LAST                                                                                                 */
    /*                                                                                                                        */
    /*  set word1 word2 word3 word4 word5 word6                                                                               */
    /*  set word1 word2 word3 word4 word5 word6                                                                               */
    /*  set word1 word2 word3 word4 word5 word6                                                                               */
    /*  set word1 word2 word3 word4 word5 word6                                                                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
When a sentence is spread over many lines combine lines into one record sas infile python
