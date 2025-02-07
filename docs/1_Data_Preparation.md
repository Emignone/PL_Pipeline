# Como correr PLANTS 
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
Un ejemplo para descargar del config file [aca](plantsconfig1) <br>

Vamos a tener que crear N config files (N siendo el numero de cores) ya que en cada uno vamos a modificar el nombre de la base de datos. Preferentemente es ideal tener una carpeta por core donde ponemos cada config file ahi  es donde vamos a encontar el output ordenado por cada corrida. <br>

Determine the binding_site definition with the command: plants --mode bind lig.mol2 rec.mol2 <br>
Copy info bindingsite_center and bidingsite_radius into dock_opsi3.txt: <br>
bindingsite_center 22.7142 -25.2627 -3.283 <br>
bindingsite_radius 3.75624 <br>
### Sin aguas
```
# scoring function and search settings
scoring_function chemplp
search_speed speed1


# input
protein_file Path_to/rec.mol2 
ligand_file Path_to/ligand_1.mol2
# output
output_dir results1 
write_protein_conformations 0
write_ranking_multi_mol2 1 #Esta linea solo aca puede esar descomentada

# write single mol2 files (e.g. for RMSD calculation)
write_multi_mol2 1

# binding site definition
bindingsite_center 10.631 -60.854 -0.102727 
bindingsite_radius 9 


# cluster algorithm
cluster_structures 10
cluster_rmsd 2.0

```

### Con aguas

```
# scoring function and search settings
scoring_function chemplp
search_speed speed1


# input
protein_file Path_to/rec.mol2 
ligand_file Path_to/ligand_1.mol2
# output
output_dir results1 
write_protein_conformations 0
#write_ranking_multi_mol2 1  #Cuando hay aguas, PLANTS no permite usar esta funcion de output

# write single mol2 files (e.g. for RMSD calculation)
write_multi_mol2 1

# binding site definition
bindingsite_center 10.631 -60.854 -0.102727 
bindingsite_radius 9 


# cluster algorithm
cluster_structures 10
cluster_rmsd 2.0

#water model
water_molecule 13.7092 -60.1224 7.85174 0 0 
water_molecule_definition water.mol2
water_protein_hb_weight 0.0
water_water_hb_weight 0.0
water_enable_penalty 0.0

```

## Correr el docking
Para esto, vamos a tener que correr el siguiente comando, uno por cada core:
```
Path_to/PLANTS1.2_64bit --mode screen plantsconfig > target_n.log &
```


