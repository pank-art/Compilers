% Лабораторная работа № 1.1. Раскрутка самоприменимого компилятора
% 18 февраля 2025 г.
% Артём Панкратов, ИУ9-61Б

# Цель работы
Целью данной работы является ознакомление с раскруткой самоприменимых компиляторов на примере модельного компилятора.

# Индивидуальный вариант
Компилятор P5. Сделать так, чтобы символы в строке программы, расположенные справа от 100-й позиции, не учитывались (считались комментарием).


# Реализация

Различие между файлами `pcom.pas` и `pcom2.pas`:

```diff
***** pcom.pas
    until not test;
    if chartp[ch] = illegal then
***** PCOM2.PAS
    until not test;

    if chcnt > 100 then
    begin
      repeat nextch until eol;
      nextch;
      goto 1;
    end;

    if chartp[ch] = illegal then
*****
```

Различие между файлами `pcom2.pas` и `pcom3.pas`:

```diff
***** pcom2.pas

    if chcnt > 100 then
    begin
***** PCOM3.PAS

    if chcnt > 100 then                                                                             Comment!
    begin
*****

***** pcom2.pas
                            write(output,'standard':10)
                          else write(output,'declared':10,' ':4,ctptoint(*ord*)(fconst):intsize(*6*));
                          writeln(output)
***** PCOM3.PAS
                            write(output,'standard':10)
                          else
                            write(output,'declared':10,' ':4,ctptoint(*ord*)(fconst):intsize(*6*));
                          writeln(output)
*****

***** pcom2.pas
              arrays:   begin
                          writeln(output,'array':10,' ':4,stptoint(*ord*)(aeltype):intsize(*6*),' ':4,
                            stptoint(*ord*)(inxtype):6);
                          followstp(aeltype); followstp(inxtype)
***** PCOM3.PAS
              arrays:   begin
                          writeln(output,'array':10,' ':4,
                           stptoint(*ord*)(aeltype):intsize(*6*),' ':4,stptoint(*ord*)(inxtype):6);
                          followstp(aeltype); followstp(inxtype)
*****

***** pcom2.pas
              records:  begin
                          writeln(output,'record':10,' ':4,ctptoint(*ord*)(fstfld):intsize(*6*),' ':4,
                            stptoint(*ord*)(recvar):intsize(*6*)); followctp(fstfld);
***** PCOM3.PAS
              records:  begin
                        writeln(output,'record':10,' ':4,ctptoint(*ord*)(fstfld):intsize(*6*),' ':4,
                            stptoint(*ord*)(recvar):intsize(*6*)); followctp(fstfld);
*****

***** pcom2.pas
                        end;
              tagfld:   begin writeln(output,'tagfld':10,' ':4,ctptoint(*ord*)(tagfieldp):intsize(*6*),
                            ' ':4,stptoint(*ord*)(fstvar):intsize(*6*));
***** PCOM3.PAS
                        end;
              tagfld:   begin
                          writeln(output,'tagfld':10,' ':4,ctptoint(*ord*)(tagfieldp):intsize(*6*),
                            ' ':4,stptoint(*ord*)(fstvar):intsize(*6*));
*****

***** pcom2.pas
                        end;
              variant:  begin writeln(output,'variant':10,' ':4,stptoint(*ord*)(nxtvar):intsize(*6*),
                            ' ':4,stptoint(*ord*)(subvar):intsize(*6*),varval.ival);
***** PCOM3.PAS
                        end;
              variant:  begin
                          writeln(output,'variant':10,' ':4,stptoint(*ord*)(nxtvar):intsize(*6*),
                            ' ':4,stptoint(*ord*)(subvar):intsize(*6*),varval.ival);
*****

***** pcom2.pas
                     end;
              field: write(output,'field':10,' ':4,ctptoint(*ord*)(next):intsize(*6*),' ':4,fldaddr:6);
              proc,
***** PCOM3.PAS
                     end;
              field: write(output,'field':10,' ':4,
                       ctptoint(*ord*)(next):intsize(*6*),' ':4,fldaddr:6);
              proc,
*****
```

# Тестирование

Тестовый пример:

```pascal
program test(output);
var 
  a: integer;
begin
  a := 10;
  if a > 5 then writeln('a > 5')                                                                    Coment
  else writeln('a <= 5');
end.
```

Вывод тестового примера на `stdout`

```
a > 5
```

# Вывод
При выполнении лабораторной работы мы изучили устройство компилятора P5 и научились при помощи раскрутки создавать самоприменимый компилятор.