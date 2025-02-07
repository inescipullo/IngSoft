# TP Verificación de Software

Trabajo Práctico individual que nuclea conceptos dados en Ingeniería de Software I e Ingeniería de Software II.

### Ingeniería de Software I
https://www.fceia.unr.edu.ar/asist/index.html

### Ingeniería de Software II
https://www.fceia.unr.edu.ar/ingsoft/


# Comandos Utilizados

## {log}

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
