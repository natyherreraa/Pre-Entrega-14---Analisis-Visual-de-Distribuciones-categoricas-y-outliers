# Pre-Entrega 14 - Analisis Visual de Distribuciones categoricas y outliers
Tecnicas de ingenieria de datos para caracterizar variables. 

## Objetivo

Describir cómo se distribuye la cartera de seguros de automotores de 2024 por tipo de cobertura y frecuencia de pago. También se analiza la distribución de la edad del conductor y del premio total para identificar valores atípicos y decidir qué registros conviene revisar.

## Datos

El archivo `data/polizas.csv` contiene 168.133 transacciones de 2024 y conserva las 47 columnas del dataset original. Para este análisis se seleccionaron cuatro variables. Se utiliza `insured_id` como índice para identificar los registros en las tablas de resultados.

| Variable | Descripción | Tipo |
|---|---|---|
| `policy_type` | Tipo de cobertura contratada | Categórica |
| `payment_frequency` | Frecuencia de pago | Categórica |
| `driver_age` | Edad del conductor en años | Numérica discreta |
| `total_premium` | Premio total de la transacción | Numérica continua |

Fuente de los datos: Espinosa, Lledó y Atance (2026), *A detailed dataset of motor insurance policies with coverage-specific financial information*, versión 1. [Mendeley Data](https://doi.org/10.17632/sw4jmdb2sm.1) · [Licencia CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

Link de descarga de dataset: https://drive.google.com/file/d/1H2_T2xycHODlPM7hkc1sbxweu6Q7ZE23/view?usp=sharing


## Cómo ejecutar el notebook

1. Clonar o descargar el repositorio y abrir la carpeta del proyecto
2. Desde la carpeta del proyecto, crear un entorno virtual de Python llamado `.venv` y activarlo según el sistema operativo. Con el entorno activo, instalar las dependencias con el siguiente comando:

   ```bash
   python -m pip install -r requirements.txt
   ```

3. Abrir `analisis_distribuciones.ipynb`, seleccionar el kernel de `.venv` y elegir **Ejecutar todo**. El notebook lee el CSV incluido en `data/polizas.csv`.

## Análisis y resultados

- **Composición de la cartera.** Los gráficos de barras muestran que la cobertura CC concentra el 57,4 % de las transacciones. Esta categoría combina responsabilidad civil frente a terceros con dos o más coberturas adicionales. En frecuencia de pago predomina el código A, con el 83,3 %.
- **Edad del conductor.** La mediana es de 49 años y la mitad central de los registros se encuentra entre 40 y 59 años. La asimetría es levemente positiva (0,227) y el exceso de curtosis es −0,842. El diagrama de violín muestra dónde se concentran las edades y el boxplot permite ubicar los casos extremos.
- **Premio total.** La mediana es 266,38 y la media es 303,55. El boxplot muestra una cola hacia los importes altos, coherente con la asimetría positiva (2,456) y el exceso de curtosis elevado (14,985).

## Detección de outliers y decisión

Se calculó el rango intercuartílico (`IQR = Q3 − Q1`) por separado para la edad y el premio. Se identificaron como atípicos los valores inferiores a `Q1 − 1,5 × IQR` o superiores a `Q3 + 1,5 × IQR`.

| Variable | Límites IQR | Casos detectados |
|---|---|---|
| Edad del conductor | 11,5 a 87,5 años | Cuatro registros por encima del límite superior: 88, 88, 89 y 90 años. |
| Premio total | −194,63 a 743,88 | 7.075 registros por encima del límite superior (4,21 %). El mínimo observado es 0. |

Se conservaron los registros en su escala original. Las cuatro edades son posibles y se podrían validar con la fecha de nacimiento del conductor y la fecha de la transacción. Para los premios altos, se propone comparar los importes entre transacciones del mismo tipo de cobertura y frecuencia de pago antes de corregir un registro. Para describir el valor habitual de la cartera se utiliza la mediana, que es menos sensible a los extremos.