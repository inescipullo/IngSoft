# TP Verificación de Software

Trabajo Práctico individual que nuclea conceptos dados en Ingeniería de Software I e Ingeniería de Software II.
Consiste en plantear los requerimientos de un problema, dar la especificación en Z, simulaciones y demostraciones de lemas de invarianza con {log} y Z/Eves, y testing con Fastest.

### Ingeniería de Software I
https://www.fceia.unr.edu.ar/asist/index.html

### Ingeniería de Software II
https://www.fceia.unr.edu.ar/ingsoft/


# Comandos Utilizados

## {log}

{log} Version 4.9.8 Release 15h

SWI-Prolog version 9.2.9 for x86_64-linux

```
$ swipl
?- consult('setlog').
?- setlog.
{log}=> type_check.
{log}=> consult('planificador.slog').
<!-- ***SIMULACIONES*** -->
{log}=> vcg('planificador.slog').
{log}=> consult('planificador-vc.slog').
{log}=> check_vcs_planificador.
```

## Z/Eves

Está en el labdcc, no vale la pena intentar descargarlo :)

```
$ z-eves
=> read "zEves.tex";
=> try InvDispatcher \land TerminateProcess \implies InvDispatcher';
=> invoke TerminateProcess;
=> split TerminateProcessOk;
=> simplify;
=> cases;
=> invoke TerminateProcessOk;
=> invoke InvDispatcher;
=> equality substitute procQueue';
=> simplify;
=> next;
=> invoke TerminateProcessError;
=> invoke InvDispatcher;
=> invoke \Xi Dispatcher;
=> rewrite;
=> next;
```

## Fastest

Fastest Version 1.7

```
$ java -jar fastest.jar
Fastest> loadspec ../fastest.tex 
Fastest> selop NewProcess
Fastest> genalltt
Fastest> showtt
Fastest> addtactic NewProcess_DNF_1 SP \notin p? \notin \ran procQueue
Fastest> addtactic NewProcess_DNF_3 SP \in p? \in \ran procQueue
Fastest> genalltt
Fastest> showtt -o arboltt.tex
Fastest> showsch -tcl -o esquemastcl.tex
Fastest> genalltca
Fastest> showtt -o arboltca.tex
Fastest> showsch -tca -o esquemastca.tex
```
