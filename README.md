# TP Verificación de Software

Trabajo Práctico individual que nuclea conceptos dados en Ingeniería de Software I e Ingeniería de Software II.
Consiste en plantear los requerimientos de un problema, dar la especificación en Z, simulaciones y demostraciones de lemas de invarianza con {_log_} y Z/Eves, y testing con Fastest.

### Ingeniería de Software I
https://www.fceia.unr.edu.ar/asist/index.html

### Ingeniería de Software II
https://www.fceia.unr.edu.ar/ingsoft/


# Comandos de {_log_} Utilizados

>[!NOTE]
> {log} Version 4.9.9 Release 1d
> 
> SWI-Prolog version 9.3.29 for x86_64-linux

## Para verificar que el programa este correctamente tipado (pasa el _typechecker_)

```
$ swipl
?- consult('setlog').
?- setlog.
{log}=> type_check.
{log}=> consult('planificador.slog').
```

## Para ejecutar las simulaciones en el entorno NEXT

```
$ swipl
?- consult('setlog').
?- setlog.
{log}=> consult('planificador.slog').
{log}=> vcg('planificador.slog').
{log}=> consult('planificador-vc.slog').
{log}=> setpp(quantum).
```

Y luego ejecutar las simulaciones.
El comando `setpp(quantum).` es porque para que la ejecución de simulaciones dentro del entorno NEXT funcione hay que darle explicitamente la definición de los parametros (ver en manual de {_log_}).
No pareció ser necesario descargar exitosamente las condiciones de verificación.

## Para generar casos de prueba y descargarlos (VCG)

```
$ swipl
?- consult('setlog').
?- setlog.
{log}=> consult('planificador.slog').
{log}=> vcg('planificador.slog').
{log}=> consult('planificador-vc.slog').
{log}=> check_vcs_planificador.
```

Antes de `consult('planificador-vc.slog').`, hacer cambios en el archivo `planificador-vc.slog` de ser necesario, para que todos los casos de prueba se descarguen exitosamente.

## Para generar casos de prueba usando {_log_}-TTF

```
$ swipl
?- consult('setlog').
?- setlog.
{log}=> type_check.
{log}=> consult('planificador.slog').
{log}=> consult('planificador-vc.slog').
{log}=> check_vcs_planificador.
{log}=> ttf(planificador).
```

El comando `ttf(planificador)` es para inicializar {_log_}-TTF, y despues los comandos para generar los casos de prueba de cada operación. Luego de aplicar un comando `applydnf` sobre una operación, todas las subsecuentes tácticas serán aplicadas sobre esa misma aplicación. Para reiniciar el proceso de generación de tests (para generar tests sobre la misma u otra operación), se debe ejecutar el comando `ttf` nuevamente.

### Operación `newProcess`:
```
{log}=> ttf(planificador).
{log}=> applydnf(newProcess(NewProc)).
{log}=> writett.
{log}=> applysp(newProcess_dnf_1,un(ProcQueue, {[Max1, NewProc]}, ProcQueue_)).
{log}=> prunett.
{log}=> writett.
{log}=> gentc.
{log}=> writetc.
{log}=> exporttt.
```

### Operación `dispatch`:
```
{log}=> ttf(planificador).
{log}=> applydnf(dispatch).
{log}=> writett.
{log}=> applysp(dispatch_dnf_2,un(ProcQueue, {[Max1, Current]}, ProcQueue_)).
{log}=> prunett.
{log}=> writett.
{log}=> applysp(dispatch_dnf_5,diff(ProcQueue, {[Min, Head]}, ProcQueue_)).
{log}=> prunett.
{log}=> writett.
{log}=> gentc.
{log}=> writetc.
{log}=> exporttt.
```

### Operación `terminateProc`:
```
{log}=> ttf(planificador).
{log}=> applydnf(terminateProc(Proc)).
{log}=> writett.
{log}=> gentc.
{log}=> writett.
{log}=> writetc.
{log}=> exporttt.
```
