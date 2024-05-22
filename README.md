20205467603|CE01|030301|30.0|0|recruc2|1997-08-15 13:15:31|
20205467603|CE02|010101|19.0|0|reru0499|2008-04-18 16:43:47|
20205467603|CE03|010101|19.0|0|reru0499|2008-04-18 16:48:09|

0071|20205467603|CE01|1|D.LEG.662|08/29/1991|06/15/1994|06/14/2004|0|2|3|-|-|rerupri7|2004-09-17 12:26:20|
0071|20205467603|CE02|0|27623|12/13/2001|09/01/2003|05/01/2004|0|-|1|1| |reru0499|2008-04-18 16:43:47|
0071|20205467603|CE03|0|27623|12/13/2001|01/01/2005|12/31/2006|0|-|1|1| |reru0499|2008-04-18 16:48:09|

========================
{
	"_id" : ObjectId("6453066ce8d3815dc6911b4c"),
	"numRuc" : "20205467603",
	"codDependencia" : "0011",
	"lisConvenio" : [
		{
			"codConvenio" : "CE01",
			"indEstado" : 1,
			"desDispositivoConvenio" : "D.LEG.662",
			"fecPromulgacion" : ISODate("1991-08-29T00:00:00.000-05:00"),
			"fecInicioConvenio" : ISODate("1994-06-15T00:00:00.000-05:00"),
			"fecFinConvenio" : ISODate("2004-06-14T00:00:00.000-05:00"),
			"indReinversionUtilidades" : 0,
			"indMotivoRenuncia" : "2",
			"codTipoConvenio" : 3,
			"indContabilidad" : "-",
			"codMoneda" : "-",
			"lisTributo" : [
				{
					"codTributo" : "030301",
					"porTasa" : Decimal128("30.00"),
					"indMarcaExoneracion" : "0",
					"auditoria" : {
						"codUsuRegis" : "recruc2",
						"fecRegis" : ISODate("1997-08-15T08:15:31.000-05:00"),
						"codUsuModif" : "recruc2",
						"fecModif" : ISODate("1997-08-15T08:15:31.000-05:00")
					}
				}
			],
			"auditoria" : {
				"codUsuRegis" : "rerupri7",
				"fecRegis" : ISODate("2004-09-17T07:26:20.000-05:00"),
				"codUsuModif" : "rerupri7",
				"fecModif" : ISODate("2004-09-17T07:26:20.000-05:00")
			}
		},
		{
			"codConvenio" : "CE03",
			"indEstado" : 0,
			"desDispositivoConvenio" : "27623",
			"fecPromulgacion" : ISODate("2001-12-13T00:00:00.000-05:00"),
			"fecInicioConvenio" : ISODate("2005-01-01T00:00:00.000-05:00"),
			"fecFinConvenio" : ISODate("2006-12-31T00:00:00.000-05:00"),
			"indReinversionUtilidades" : 0,
			"indMotivoRenuncia" : "-",
			"codTipoConvenio" : 1,
			"indContabilidad" : "1",
			"codMoneda" : " ",
			"lisTributo" : [
				{
					"codTributo" : "010101",
					"porTasa" : Decimal128("19.00"),
					"indMarcaExoneracion" : "0",
					"auditoria" : {
						"codUsuRegis" : "reru0499",
						"fecRegis" : ISODate("2008-04-18T11:48:09.000-05:00"),
						"codUsuModif" : "reru0499",
						"fecModif" : ISODate("2008-04-18T11:48:09.000-05:00")
					}
				}
			],
			"auditoria" : {
				"codUsuRegis" : "reru0499",
				"fecRegis" : ISODate("2008-04-18T11:48:09.000-05:00"),
				"codUsuModif" : "reru0499",
				"fecModif" : ISODate("2008-04-18T11:48:09.000-05:00")
			}
		},
		{
			"codConvenio" : "CE02",
			"indEstado" : 0,
			"desDispositivoConvenio" : "27623",
			"fecPromulgacion" : ISODate("2001-12-13T00:00:00.000-05:00"),
			"fecInicioConvenio" : ISODate("2003-09-01T00:00:00.000-05:00"),
			"fecFinConvenio" : ISODate("2004-05-01T00:00:00.000-05:00"),
			"indReinversionUtilidades" : 0,
			"indMotivoRenuncia" : "-",
			"codTipoConvenio" : 1,
			"indContabilidad" : "1",
			"codMoneda" : " ",
			"lisTributo" : [
				{
					"codTributo" : "010101",
					"porTasa" : Decimal128("19.00"),
					"indMarcaExoneracion" : "0",
					"auditoria" : {
						"codUsuRegis" : "reru0499",
						"fecRegis" : ISODate("2008-04-18T11:43:47.000-05:00"),
						"codUsuModif" : "reru0499",
						"fecModif" : ISODate("2008-04-18T11:43:47.000-05:00")
					}
				}
			],
			"auditoria" : {
				"codUsuRegis" : "reru0499",
				"fecRegis" : ISODate("2008-04-18T11:43:47.000-05:00"),
				"codUsuModif" : "reru0499",
				"fecModif" : ISODate("2008-04-18T11:43:47.000-05:00")
			}
		}
	],
	"auditoria" : {
		"codUsuRegis" : "rerupri7",
		"fecRegis" : ISODate("2004-09-17T07:26:20.000-05:00"),
		"codUsuModif" : "reru0499",
		"fecModif" : ISODate("2008-04-18T11:48:09.000-05:00"),
		"numSchemaVersion" : "1.0"
	}
}

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
