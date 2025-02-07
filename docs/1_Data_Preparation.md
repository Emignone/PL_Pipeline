Para poder correr docking con PLANTS vamos a necesitar: <br>
1. Dos ejecutables: que se pueden encontrar en [ejecutables](https://github.com/Emignone/PL_Pipeline/edit/main/Ejecutables/)<br>
  a. SPORES <br>
  b. PLANTS <br>
  
## Receptor
En formato .mol2. <br>
Hay que ser cuidadoso si tiene agua. Si la tiene el procediemitno es un poco diferente (cambia el config file) y es importante separar el agua en un archivo .mol2 diferente <br>

Si no es manual (con ICM), se puede hacer: <br>
```
spores --mode splitpdb protein.pdb
```
Lo que hace es separar proteina, ligando y aguas. Pero como es con .pdb no va a estar prepaado el .mol2 y va a tener que ser preparado,.

## Base de datos
En formato .mol2, pero hay que pasarla por spores antes de usarla, por eso lo ideal es hacer lo siguiente: <br>
1. Dividirla en la cantidad de cores a usar
2. Correr el siguiente comando con SPORES:
   ```
   Path_to/./SPORES_64bit --mode readbonds target_a.mol2 target1.mol2
   ```
   Usamos la logica de ir correlacionados alfabeticamente con los numeros.

## Config file
Determine the binding_site definition with the command: plants --mode bind lig.mol2 rec.mol2 <br>
Copy info bindingsite_center and bidingsite_radius into dock_opsi3.txt: <br>
bindingsite_center 22.7142 -25.2627 -3.283 <br>
bindingsite_radius 3.75624 <br>
### Sin aguas

### Con aguas
