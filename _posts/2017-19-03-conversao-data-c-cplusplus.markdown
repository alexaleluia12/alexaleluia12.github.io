---
layout: post
title:  "conversão de data c/c++"
date:   2017-03-19 18:57:31 -0300
categories: meublog
---

Conversão de data em formato legivel "YYYY-mm-dd" para uma representação em timestamp.<br>
Tinha usado boost no começo mas, não funcionava em para algumas datas. Usando as próprias funções do C funcionou sem falhas.

```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

// *********************
/*
arquivo => e.c
compilar
  $ make e
rodar
  $ ./e
*/
// *********************

/**
 * Converte data no formato americano para timestamp (segundos desde 1970)
 * date deve estar no formato 'YYYY-mm-dd'
 */
time_t date_to_timestamp(char* date)
{
  struct tm tm;
  memset(&tm, 0, sizeof(struct tm));
  strptime(date, "%Y-%m-%d", &tm);

  tm.tm_isdst = 1; // inclui o horario de verao no calculo

  time_t data_timestump = mktime(&tm);
  return data_timestump;
}



int main (int argc, char const* argv[])
{
  printf("%ld \n", date_to_timestamp("2010-11-28"));
  /*
  eh o msm valor retornado pelo python3
>>> import datetime
>>> a = datetime.datetime(2010, 11, 28)
>>> a.timestamp()
1290909600.0
>>> 
  */
  return 0;
}
```

Inverso da anterior. Timestamp para data. :D

```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

// *********************
/*
arquivo => f.c
compilar
  $ make f
rodar
  $ ./f
*/
// *********************

/**
 * 
 * Converte timestamp para data.
 * (obs. retorna nada devido a alocao de memoria, o que deixaria a funcao mais complexa)
 */
void timestamp_to_date(time_t ts)
{
  struct tm * datae;
  datae = gmtime(&ts);

  
  char buf[255];
  strftime(buf, sizeof(buf), "%F", datae);
  puts(buf);
}



int main (int argc, char const* argv[])
{
  timestamp_to_date(1290909600);

  return 0;
}

```
