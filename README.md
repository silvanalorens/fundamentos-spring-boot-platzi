#!/bin/bash

# Nombre de los archivos de entrada
input_cab_file="inputcab.txt"
input_det_file="inputdetalle.txt"
# Nombre del archivo de salida
output_file="output.json"

# Inicializa arrays para almacenar los registros de clientes y ventas
declare -A ventas

# Leer el archivo de detalles línea por línea
while IFS='|' read -r numRucDetalle idclienteDetalle ventasDetalle _; do
    ventas[$idclienteDetalle]=$ventasDetalle
done < "$input_det_file"

# Crear el archivo JSON con la estructura deseada
{
  echo "{"
  
  # Leer el archivo de cabecera para obtener numRuc y ciudad
  IFS='|' read -r numRuc ciudad _ < "$input_cab_file"
  echo '  "numRuc": "'$numRuc'",'
  echo '  "ciudad": "'$ciudad'",'
  echo '  "listcliente": ['

  # Leer el archivo de cabecera línea por línea
  first=1
  while IFS='|' read -r numRuc ciudad nombre birthday regDate idcliente email _; do
      # Eliminar posibles espacios adicionales
      nombre=$(echo "$nombre" | xargs)
      birthday=$(echo "$birthday" | xargs)
      regDate=$(echo "$regDate" | xargs)
      idcliente=$(echo "$idcliente" | xargs)
      email=$(echo "$email" | xargs)

      # Crear un registro de cliente en formato JSON
      cliente=$(cat <<EOF
{
  "idcliente": "$idcliente",
  "nombre": "$nombre",
  "birthday": "$birthday",
  "regDate": "$regDate",
  "correo": "$email",
  "listventas": [
    {
      "ventas": ${ventas[$idcliente]}
    }
  ]
}
EOF
)
      # Añadir el registro de cliente al array
      if [[ $first -ne 1 ]]; then
          echo "    ,"
      fi
      echo "    $cliente"
      first=0
  done < "$input_cab_file"

  echo '  ]'
  echo "}"
} > "$output_file"

echo "Conversión completa. JSON guardado en $output_file."
