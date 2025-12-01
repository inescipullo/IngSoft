# TP Verificación de Software

Trabajo Práctico individual que nuclea conceptos dados en Ingeniería de Software I e Ingeniería de Software II.
Consiste en plantear los requerimientos de un problema, dar la especificación en Z y el correspondiente programa en {_log_} (setlog). Luego ejecutar simulaciones en el entorno NEXT (de {_log_}), generar y descargar condiciones de verificación usando el VCG (herramienta de {_log_}) y generar casos de prueba para todas las operaciones del programa utilizando {_log_}-TTF. 

### Ingeniería de Software I
https://www.fceia.unr.edu.ar/asist/index.html

### Ingeniería de Software II
https://www.fceia.unr.edu.ar/ingsoft/

# Versiones

- {log}: Version 4.9.9 Release 1d
- SWI-Prolog: version 9.3.29 for x86_64-linux
- Z/EVES: This is the Z/EVES System, version 2.3

>[!NOTE]
> Z/EVES está en el labdcc, en mi opinión no vale la pena intentar descargarlo :)

# Comandos de {_log_} utilizados

## Para verificar que el programa este correctamente tipado (pasa el _typechecker_)

```
$ swipl
?- consult('setlog.pl').
?- setlog.
{log}=> type_check.
{log}=> consult('planificador.slog').
```

## Para generar condiciones de verificación con el VCG y descargarlas

```
$ swipl
?- consult('setlog.pl').
?- setlog.
{log}=> consult('planificador.slog').
{log}=> vcg('planificador.slog').
{log}=> consult('planificador-vc.slog').
{log}=> check_vcs_planificador.
```

Antes de `consult('planificador-vc.slog').`, hacer cambios en el archivo `planificador-vc.slog` de ser necesario, para que todos los casos de prueba se descarguen exitosamente.

> Notar que de ahora en más no vuelvo a ejecutar `vcg('planificador.slog').` ya que eso regeneraría el archivo `*-vc.slog` con las condiciones de verificación, cosa que no quiero que ocurra si tuve que modificarlo para que todas las condiciones se descarguen exitosamente.

## Para ejecutar las simulaciones en el entorno NEXT

```
$ swipl
?- consult('setlog.pl').
?- setlog.
{log}=> consult('planificador.slog').
{log}=> consult('planificador-vc.slog').
{log}=> setpp(quantum).
```

Y luego ejecutar las simulaciones.
El comando `setpp(quantum).` es porque para que la ejecución de simulaciones dentro del entorno NEXT funcione hay que darle explicitamente la definición de los parametros (ver en manual de {_log_}).
No pareció ser necesario descargar exitosamente las condiciones de verificación.

### Simulación 1:
```
[initial]:[invPFun,invIny,invNat,invDispatcher] >>
newProcess(NewProc:[[p1,p2,p3]]) >> 
3:dispatch >>
terminateProc(Proc:p1) >>
8:dispatch.
```

### Simulación 2:
```
[initial]:[invPFun,invIny,invNat,invDispatcher] >>
dispatch >>
newProcess(NewProc:[[p1,p1,process:nullp]]) >> 
2:dispatch >>
terminateProc(Proc:p2) >>
5:dispatch.
```

## Para generar casos de prueba usando {_log_}-TTF

Condiciones que se deben cumplir para poder usar {_log_}-TTF:
1. La especificación ha pasado el control de tipos.
2. La especificación ha sido analizada por el VCG.
3. El archivo con las condiciones de verificación ha sido consultado.
4. Que se hayan descargado las condiciones de verificación generadas por el VCG (no necesario pero recomendable).

```
$ swipl
?- consult('setlog.pl').
?- setlog.
{log}=> type_check.
{log}=> consult('planificador.slog').
{log}=> consult('planificador-vc.slog').
{log}=> check_vcs_planificador.
```

Luego, el comando `ttf` para inicializar {_log_}-TTF, y los comandos para generar los casos de prueba de cada operación. Despues de aplicar un comando `applydnf` sobre una operación, todas las subsecuentes tácticas serán aplicadas sobre esa misma aplicación. Para reiniciar el proceso de generación de tests (para generar tests sobre la misma u otra operación), se debe ejecutar el comando `ttf` nuevamente.

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
{log}=> applyii(dispatch_vis,RemTicks,[0]).
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

# Comandos de Z/EVES

Es solo para ver que la especificación pasa el control de tipos de Z/EVES.

```
$ z-eves
=> read "zEves.tex";
```
