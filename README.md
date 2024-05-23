### FIX #############
#!/bin/bash

# Nombre de los archivos de entrada
input_cab_file="t335cabcon_20240521_20205467603.unl"
input_det_file="t337detcon_20240521_20205467603.unl"
# Nombre del archivo de salida
output_file="output.json"

# Inicializa arrays para almacenar los registros de convenios y detalle
declare -A detalle
declare numRucDetalle

# Inicializar variables para almacenar las fechas
min_fecha_proceso=""
max_fecha_actualizacion=""

min_user_na=""
min_user_act=""

# Función para formatear los valores y generar el campo "lisTributo"
function functionformatJson() {
    local codTriDetalle="$1"
    local tasaDetalle="$2"
    local marExoDetalle="$3"
    local userNaDetalle="$4"
    local fecActDetalle="$5"

    # Formatea los valores según el formato deseado
    echo '{
        "codTributo": "'"$codTriDetalle"'",
        "porTasa": Decimal128("'"$tasaDetalle"'"),
        "indMarcaExoneracion": "'"$marExoDetalle"'",
        "auditoria": {
            "codUsuRegis": "reru0499",
            "fecRegis": ISODate("'"$fecActDetalle"'"),
            "codUsuModif": "reru0499",
            "fecModif": ISODate("'"$fecActDetalle"'")
        }
    }'
}

# Leer el archivo de detalles línea por línea
while IFS='|' read -r numRucDetalle codConDetalle codTriDetalle tasaDetalle marExoDetalle userNaDetalle fecActDetalle _; do
    # Asocia los valores con las claves numRucDetalle y codConDetalle
    detalle["$numRucDetalle|$codConDetalle|2"]=$codTriDetalle
    detalle["$numRucDetalle|$codConDetalle|3"]="Decimal128(\"$tasaDetalle\")"
    detalle["$numRucDetalle|$codConDetalle|4"]=$marExoDetalle
    detalle["$numRucDetalle|$codConDetalle|5"]=$userNaDetalle
    detalle["$numRucDetalle|$codConDetalle|6"]=$(date -d "$fecActDetalle" +"%Y-%m-%dT%H:%M:%S.%3N%:z")
done < "$input_det_file"

# Crear el archivo JSON con la estructura deseada
{
  echo "{"
  
  # Leer el archivo de cabecera para obtener numRuc y ciudad
  IFS='|' read -r codDepen numRucCab _ < "$input_cab_file"
  echo '  "numRuc": "'$numRucCab'",'
  echo '  "codDependencia": "'$codDepen'",'
  echo '  "lisConvenio": ['

  # Leer el archivo de cabecera línea por línea
  first=1
  while IFS='|' read -r codDepen numRucCab codConCab estadoCab conDisCab fecProCab conDesCab conHasCab benReInCab motRenCab tipConCab mcontaCab monedaCab userNaCab fecActCab  _; do
      # Eliminar posibles espacios adicionales
      numRuc=$(echo "$numRucCab" | xargs)
      codCon=$(echo "$codConCab" | xargs)
      estado=$(echo "$estadoCab" | xargs)
      conDis=$(echo "$conDisCab" | xargs)
      fecPro=$(echo "$fecProCab" | xargs)
      fecProIso=$(TZ="America/Lima" date -d "$fecPro" +"%Y-%m-%dT%H:%M:%S.%3NZ")
      conDes=$(echo "$conDesCab" | xargs)
      yearConDes=$(date -d "$conDes" '+%Y')
      monthConDes=$(date -d "$conDes" '+%m')
      perConDes="${yearConDes}${monthConDes}"
      conDesIso=$(TZ="America/Lima" date -d "$conDes" +"%Y-%m-%dT%H:%M:%S.%3NZ")
      conHas=$(echo "$conHasCab" | xargs)
      yearConHas=$(date -d "$conHas" '+%Y')
      monthConHas=$(date -d "$conHas" '+%m')
      perConHas="${yearConHas}${monthConHas}"
      conHasIso=$( date -d "$conHas" +"%Y-%m-%dT%H:%M:%S.%3NZ")
      benReIn=$(echo "$benReInCab" | xargs)
      motRen=$(echo "$motRenCab" | xargs)
      tipCon=$(echo "$tipConCab" | xargs)
      mconta=$(echo "$mcontaCab" | xargs)
      moneda=$(echo "$monedaCab" | xargs)
      userNa=$(echo "$userNaCab" | xargs)
      fecAct=$(echo "$fecActCab" | xargs)
      fecActIso=$(TZ="America/Lima" date -d "$fecAct" +"%Y-%m-%dT%H:%M:%S.%3NZ")
      yearFecAct=$(date -d "$fecAct" '+%Y')
      monthFecAct=$(date -d "$fecAct" '+%m')
      
      perFecAct="${yearFecAct}${monthFecAct}"

      if [[ $estado -ne 0 ]]; then
          perConHas=$perFecAct
      fi

      # Si es linea 1 
      if [[ $first -eq 1 ]]; then
          min_user_na=$(echo "$userNaCab" | xargs)
          min_user_act=$(echo "$userNaCab" | xargs)
      fi

      # Convertir las fechas de texto a objetos de tipo fecha
      fecha_proceso_obj=$(date -d "$fecPro" '+%s')
      fecha_actualizacion_obj=$(date -d "$fecAct" '+%s')
  
      # Comparar y actualizar la mínima fecha de proceso
      if [[ -z "$min_fecha_proceso" || "$fecha_proceso_obj" -lt "$min_fecha_proceso" ]]; then
          min_fecha_proceso="$fecha_proceso_obj"
          min_user_na=$(echo "$userNaCab" | xargs)
      fi

      # Comparar y actualizar la máxima fecha de actualización
      if [[ -z "$max_fecha_actualizacion" || "$fecha_actualizacion_obj" -gt "$max_fecha_actualizacion" ]]; then
          max_fecha_actualizacion="$fecha_actualizacion_obj"
          min_user_act=$(echo "$userNaCab" | xargs)
      fi

      # Crear un registro de convenio en formato JSON
      itemConvenio=$(cat <<EOF
{
  "codConvenio": "$codCon",
  "indEstado": "$estado",
  "desDispositivoConvenio": "$conDis",
  "fecPromulgacion": "$fecProIso",
  "fecInicioConvenio": "$conDesIso",
  "fecFinConvenio": "$conHasIso",
  "indReinversionUtilidades": "$benReIn",
  "indMotivoRenuncia": "$motRen",
  "codTipoConvenio": "$tipCon",
  "indContabilidad": "$mconta",
  "codMoneda": "$moneda", 
  "perInicioConvenio": "$perConDes",
  "perFinConvenio": "$perConHas",
  "lisTributo": [
EOF
)

firstDetalle=1
listTributo=""
# Leer el archivo de detalles línea por línea
while IFS='|' read -r numRucDetalle codConDetalle codTriDetalle tasaDetalle marExoDetalle userNaDetalle fecActDetalle _; do
    # Filtrar las líneas que coinciden con numRucCab y codCon
    if [[ "$numRucDetalle" == "$numRuc" && "$codConDetalle" == "$codCon" ]]; then
        # Imprime la estructura del JSON
        formattedJson=$(functionformatJson "$codTriDetalle" "$tasaDetalle" "$marExoDetalle" "$userNaDetalle" "$fecActDetalle")
        
        if [[ $firstDetalle -eq 1 ]]; then
            listTributo="$formattedJson"
        else
            listTributo="$listTributo,$formattedJson"
        fi
        firstDetalle=0
    fi
done < "$input_det_file"

listTributo="$listTributo]"

itemConvenio="$itemConvenio $listTributo}"

if [[ $first -ne 1 ]]; then
    echo "    ,"
fi

echo "    $itemConvenio"
first=0

done < "$input_cab_file"

echo '  ],'
echo '  "auditoria": {'
echo '    "codUsuRegis": "'"$min_user_na"'",'
echo '    "fecRegis": "'"$(date -d "@$min_fecha_proceso" '+%Y-%m-%dT%H:%M:%S.%3N%:z')"'",
echo '    "codUsuModif": "'"$min_user_act"'",'
echo '    "fecModif": "'"$(date -d "@$max_fecha_actualizacion" '+%Y-%m-%dT%H:%M:%S.%3N%:z')"'",
echo '    "numSchemaVersion": "1.0"'
echo '  }'
echo "}"
} > "$output_file"

echo "Conversión completa. JSON guardado en $output_file."
############
#!/bin/bash

# Función para formatear los valores y generar el campo "lisTributo"
function functionformatJson() {
    local codTriDetalle="$1"
    local tasaDetalle="$2"
    local marExoDetalle="$3"
    local userNaDetalle="$4"
    local fecActDetalle="$5"

    # Formatea los valores según el formato deseado
    echo '{
        "codTributo": "'"$codTriDetalle"'",
        "porTasa": Decimal128("'"$tasaDetalle"'"),
        "indMarcaExoneracion": "'"$marExoDetalle"'",
        "auditoria": {
            "codUsuRegis": "reru0499",
            "fecRegis": ISODate("'"$fecActDetalle"'"),
            "codUsuModif": "reru0499",
            "fecModif": ISODate("'"$fecActDetalle"'")
        }
    }'
}

# Ejemplo de uso
codTriDetalle="010101"
tasaDetalle="19.00"
marExoDetalle="0"
userNaDetalle="reru0499"
fecActDetalle="2008-04-18T11:43:47.000-05:00"

formattedJson=$(functionformatJson "$codTriDetalle" "$tasaDetalle" "$marExoDetalle" "$userNaDetalle" "$fecActDetalle")
echo "$formattedJson"


===== convertplantojson2.sh ====== #!/bin/bash

# Nombre de los archivos de entrada
input_cab_file="t335cabcon_20240521_20205467603.unl"
input_det_file="t337detcon_20240521_20205467603.unl"
# Nombre del archivo de salida
output_file="output.json"

# Inicializa arrays para almacenar los registros de convenios y detalle
declare -A detalle
declare numRucDetalle

# Inicializar variables para almacenar las fechas
min_fecha_proceso=""
max_fecha_actualizacion=""

min_user_na=""
min_user_act=""

# Función para formatear los valores y generar el campo "lisTributo"
function functionformatJson() {
    local codTriDetalle="$1"
    local tasaDetalle="$2"
    local marExoDetalle="$3"
    local userNaDetalle="$4"
    local fecActDetalle="$5"

    # Formatea los valores según el formato deseado
    echo '{
        "codTributo": "'"$codTriDetalle"'",
        "porTasa": Decimal128("'"$tasaDetalle"'"),
        "indMarcaExoneracion": "'"$marExoDetalle"'",
        "auditoria": {
            "codUsuRegis": "reru0499",
            "fecRegis": ISODate("'"$fecActDetalle"'"),
            "codUsuModif": "reru0499",
            "fecModif": ISODate("'"$fecActDetalle"'")
        }
    }'
}


# Leer el archivo de detalles línea por línea
while IFS='|' read -r numRucDetalle codConDetalle codTriDetalle tasaDetalle marExoDetalle userNaDetalle fecActDetalle _; do
    # Asocia los valores con las claves numRucDetalle y codConDetalle
    detalle["$numRucDetalle|$codConDetalle|2"]=$codTriDetalle
    detalle["$numRucDetalle|$codConDetalle|3"]="Decimal128(\"$tasaDetalle\")"
    detalle["$numRucDetalle|$codConDetalle|4"]=$marExoDetalle
    detalle["$numRucDetalle|$codConDetalle|5"]=$userNaDetalle
    detalle["$numRucDetalle|$codConDetalle|6"]=$(date -d "$fecActDetalle" +"%Y-%m-%dT%H:%M:%S.%3N%:z")    
done < "$input_det_file"



# Crear el archivo JSON con la estructura deseada
{
  echo "{"
  
  # Leer el archivo de cabecera para obtener numRuc y ciudad
  IFS='|' read -r codDepen numRucCab _ < "$input_cab_file"
  echo '  "numRuc": "'$numRucCab'",'
  echo '  "codDependencia": "'$codDepen'",'
  echo '  "lisConvenio": ['

  # Leer el archivo de cabecera línea por línea
  first=1
  while IFS='|' read -r codDepen numRucCab codConCab estadoCab conDisCab fecProCab conDesCab conHasCab benReInCab motRenCab tipConCab mcontaCab monedaCab userNaCab fecActCab  _; do
      # Eliminar posibles espacios adicionales
      numRuc=$(echo "$numRucCab" | xargs)
      codCon=$(echo "$codConCab" | xargs)
      estado=$(echo "$estadoCab" | xargs)
      conDis=$(echo "$conDisCab" | xargs)
      fecPro=$(echo "$fecProCab" | xargs)
      fecProIso=$(TZ="America/Lima" date -d "$fecPro" +"%Y-%m-%dT%H:%M:%S.%3NZ")
      conDes=$(echo "$conDesCab" | xargs)
      yearConDes=$(date -d "$conDes" '+%Y')
      monthConDes=$(date -d "$conDes" '+%m')
      perConDes="${yearConDes}${monthConDes}"
      conDesIso=$(TZ="America/Lima" date -d "$conDes" +"%Y-%m-%dT%H:%M:%S.%3NZ")
      conHas=$(echo "$conHasCab" | xargs)
      yearConHas=$(date -d "$conHas" '+%Y')
      monthConHas=$(date -d "$conHas" '+%m')
      perConHas="${yearConHas}${monthConHas}"      
      conHasIso=$( date -d "$conHas" +"%Y-%m-%dT%H:%M:%S.%3NZ")
      benReIn=$(echo "$benReInCab" | xargs)
      motRen=$(echo "$motRenCab" | xargs)
      tipCon=$(echo "$tipConCab" | xargs)         
      mconta=$(echo "$mcontaCab" | xargs)
      moneda=$(echo "$monedaCab" | xargs)
      userNa=$(echo "$userNaCab" | xargs)
      fecAct=$(echo "$fecActCab" | xargs)
      fecActIso=$(TZ="America/Lima" date -d "$fecAct" +"%Y-%m-%dT%H:%M:%S.%3NZ")
      yearFecAct=$(date -d "$fecAct" '+%Y')
      monthFecAct=$(date -d "$fecAct" '+%m')
      
      perFecAct="'${yearFecAct}${monthFecAct}'"

      if [[ $estado -ne 0 ]]; then
          perConHas=$perFecAct
      fi

      # Si es linea 1 
      if [[ $first -eq 1 ]]; then
          min_user_na=$(echo "$userNaCab" | xargs)
          min_user_act=$(echo "$userNaCab" | xargs)
      fi

      # Convertir las fechas de texto a objetos de tipo fecha
      fecha_proceso_obj=$(date -d "$fecPro" '+%s')
      fecha_actualizacion_obj=$(date -d "$fecAct" '+%s')
  
      # Comparar y actualizar la mínima fecha de proceso
      if [[ -z "$min_fecha_proceso" || "$fecha_proceso_obj" -lt "$min_fecha_proceso" ]]; then
          min_fecha_proceso="$fecha_proceso_obj"
          min_user_na=$(echo "$userNaCab" | xargs)
      fi

      # Comparar y actualizar la máxima fecha de actualización
      if [[ -z "$max_fecha_actualizacion" || "$fecha_actualizacion_obj" -gt "$max_fecha_actualizacion" ]]; then
          max_fecha_actualizacion="$fecha_actualizacion_obj"
          min_user_act=$(echo "$userNaCab" | xargs)
      fi

      
      


      # Crear un registro de convenio en formato JSON
      itemConvenio=$(cat <<EOF

{
  "codConvenio": "$codCon",
  "indEstado": "$estado",
  "desDispositivoConvenio": "$conDis",
  "fecPromulgacion": "$fecProIso",
  "fecInicioConvenio": "$conDesIso",
  "fecFinConvenio": "$conHasIso",
  "indReinversionUtilidades": "$benReIn",
  "indMotivoRenuncia": "$motRen",
  "codTipoConvenio": "$tipCon",
  "indContabilidad": "$mconta",
  "codMoneda": "$moneda", 
  "perInicioConvenio": "$perConDes" ,
  "perFinConvenio": "$perConHas" ,
  "lisTributo": [
EOF
)

firstDetalle=1
listTributo=""
# Leer el archivo de detalles línea por línea
while IFS='|' read -r numRucDetalle codConDetalle codTriDetalle tasaDetalle marExoDetalle userNaDetalle fecActDetalle _; do
    # Filtrar las líneas que coinciden con numRucCab y codCon
    if [[ "$numRucDetalle" == "$numRuc" && "$codConDetalle" == "$codCon" ]]; then
        # Asocia los valores con las claves correspondientes
        detalle["$numRucDetalle|$codConDetalle|2"]=$codTriDetalle
        detalle["$numRucDetalle|$codConDetalle|3"]="Decimal128(\"$tasaDetalle\")"
        detalle["$numRucDetalle|$codConDetalle|4"]=$marExoDetalle
        detalle["$numRucDetalle|$codConDetalle|5"]=$userNaDetalle
        detalle["$numRucDetalle|$codConDetalle|6"]=$(date -d "$fecActDetalle" +"%Y-%m-%dT%H:%M:%S.%3N%:z")
        # Imprime la estructura del JSON
        formattedJson=$(functionformatJson "$codTriDetalle" "$tasaDetalle" "$marExoDetalle" "$userNaDetalle" "$fecActDetalle")
        detalle["$numRucDetalle|$codConDetalle|0"]=$formattedJson
         
    fi
    
    

    if [[ $firstDetalle -eq 1 ]]; then
        listTributo="${itemTributo}"
    else
        listTributo="${listTributo},${itemTributo}"
    fi
      firstDetalle=0

done < "$input_det_file"
listTributo="${listTributo}]"

      # Convertir las fechas de tipo fecha a formato legible
        min_fecha_proceso_readable=$(date -d "@$min_fecha_proceso" '+%Y-%m-%dT%H:%M:%S.%3N%:z')
        max_fecha_actualizacion_readable=$(date -d "@$max_fecha_actualizacion" '+%Y-%m-%dT%H:%M:%S.%3N%:z')
      # Añadir el registro de cliente al array
      itemConvenio="${itemConvenio} ${listTributo
      if [[ $first -ne 1 ]]; then
          echo "    ,"
      fi
      echo "    $itemConvenio"
      first=0
  done < "$input_cab_file"

  echo '  ],'
    echo '  "auditoria": {'
    echo '    "codUsuRegis": "'"$min_user_na"'",'
    echo '    "fecRegis": "'"$min_fecha_proceso_readable"'",'
    echo '    "codUsuModif": "'"$min_user_act"'",'
    echo '    "fecModif": "'"$max_fecha_actualizacion_readable"'",'
    echo '    "numSchemaVersion": "1.0"'
  echo "}"
} > "$output_file"

echo "Conversión completa. JSON guardado en $output_file."  =======
#!/bin/bash

# Nombre de los archivos de entrada
input_cab_file="t335cabcon_20240521_20205467603.unl"
input_det_file="t337detcon_20240521_20205467603.unl"

# Nombre del archivo de salida
output_file="output.json"

# Inicializa arrays para almacenar los registros de clientes y detalle
declare -A detalle
declare -A lisTributos
declare numRucDetalle

# Leer el archivo de detalles línea por línea
while IFS='|' read -r numRucDetalle codConDetalle codTriDetalle tasaDetalle marExoDetalle userNaDetalle fecActDetalle _; do
    # Asocia los valores con las claves numRucDetalle y codConDetalle
    detalle["$numRucDetalle|$codConDetalle|2"]=$codTriDetalle
    detalle["$numRucDetalle|$codConDetalle|3"]="Decimal128(\"$tasaDetalle\")"
    detalle["$numRucDetalle|$codConDetalle|4"]=$marExoDetalle
    detalle["$numRucDetalle|$codConDetalle|5"]=$userNaDetalle
    detalle["$numRucDetalle|$codConDetalle|6"]=$(date -d "$fecActDetalle" +"%Y-%m-%dT%H:%M:%S.%3N%:z")    
done < "$input_det_file"


# Crear el archivo JSON con la estructura deseada
{
  echo "{"
  
  # Leer el archivo de cabecera para obtener numRuc y ciudad
  IFS='|' read -r codDepen numRucCab _ < "$input_cab_file"
  echo '  "numRuc": "'$numRucCab'",'
  echo '  "codDependencia": "'$codDepen'",'
  echo '  "lisConvenio": ['

  # Leer el archivo de cabecera línea por línea
  first=1
  while IFS='|' read -r codDepen numRucCab codConCab estadoCab conDisCab fecProCab conDesCab conHasCab benReInCab motRenCab tipConCab mcontaCab monedaCab userNaCab fecActCab  _; do
      # Eliminar posibles espacios adicionales
      numRuc=$(echo "$numRucCab" | xargs)
      codCon=$(echo "$codConCab" | xargs)
      estado=$(echo "$estadoCab" | xargs)
      conDis=$(echo "$conDisCab" | xargs)
      fecPro=$(echo "$fecProCab" | xargs)
      fecProIso=$(TZ="America/Lima" date -d "$fecPro" +"%Y-%m-%dT%H:%M:%S.%3NZ")
      conDes=$(echo "$conDesCab" | xargs)
      yearConDes=$(date -d "$conDes" '+%Y')
      monthConDes=$(date -d "$conDes" '+%m')
      perConDes="${yearConDes}${monthConDes}"
      conDesIso=$(TZ="America/Lima" date -d "$conDes" +"%Y-%m-%dT%H:%M:%S.%3NZ")
      conHas=$(echo "$conHasCab" | xargs)
      yearConHas=$(date -d "$conHas" '+%Y')
      monthConHas=$(date -d "$conHas" '+%m')
      perConHas="${yearConHas}${monthConHas}"      
      conHasIso=$( date -d "$conHas" +"%Y-%m-%dT%H:%M:%S.%3NZ")
      benReIn=$(echo "$benReInCab" | xargs)
      motRen=$(echo "$motRenCab" | xargs)
      tipCon=$(echo "$tipConCab" | xargs)         
      mconta=$(echo "$mcontaCab" | xargs)
      moneda=$(echo "$monedaCab" | xargs)
      userNa=$(echo "$userNaCab" | xargs)
      fecAct=$(echo "$fecActCab" | xargs)
      fecActIso=$(TZ="America/Lima" date -d "$fecAct" +"%Y-%m-%dT%H:%M:%S.%3NZ")
      yearFecAct=$(date -d "$fecAct" '+%Y')
      monthFecAct=$(date -d "$fecAct" '+%m')
      
      perFecAct="${yearFecAct}${monthFecAct}"
       
      if [[ $estado -ne 0 ]]; then
          perConHas=$perFecAct
      fi

      # Crear un registro de cliente en formato JSON
      itemConvenio=$(cat <<EOF
{
  "codConvenio": "$codCon",
  "indEstado": "$estado",
  "desDispositivoConvenio": "$conDis",
  "fecPromulgacion": "$fecProIso",
  "fecInicioConvenio": "$conDesIso",
  "fecFinConvenio": "$conHasIso",
  "indReinversionUtilidades": "$benReIn",
  "indMotivoRenuncia": "$motRen",
  "codTipoConvenio": "$tipCon",
  "indContabilidad": "$mconta",
  "codMoneda": "$moneda", 
  "perInicioConvenio": "$perConDes" ,
  "perFinConvenio": "$perConHas" ,
  "lisTributo": $lisTributos
}
EOF
)
      # Añadir el registro de cliente al array
      if [[ $first -ne 1 ]]; then
          echo "    ,"
      fi
      echo "    $itemConvenio"
      first=0
  done < "$input_cab_file"

  echo '  ]'
  echo "}"
} > "$output_file"

echo "Conversión completa. JSON guardado en $output_file."
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
