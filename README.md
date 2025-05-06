# Apuntes de Redes - Direcciones IPv4 y Subnetting

---

## **Clases de Direcciones IPv4**
| Clase | Rango (Primer Octeto) | Máscara por Defecto | N° Redes/Hosts | Uso |
|--------|----------------------|---------------------|----------------|-----|
| A      | 1 - 126              | 255.0.0.0 (/8)      | 126 redes / 16.7M hosts | Grandes redes |
| B      | 128 - 191            | 255.255.0.0 (/16)   | 16K redes / 65K hosts | Medianas redes |
| C      | 192 - 223            | 255.255.255.0 (/24) | 2M redes / 254 hosts | Pequeñas redes |
| D      | 224 - 239            | N/A                 | Multicast       | No asignable |
| E      | 240 - 255            | N/A                 | Experimental    | Reservada |

---

## **Direcciones Privadas**
- **Clase A**: `10.0.0.0` a `10.255.255.255`
- **Clase B**: `172.16.0.0` a `172.31.255.255`
- **Clase C**: `192.168.0.0` a `192.168.255.255`

**Nota**: Las direcciones fuera de estos rangos son públicas (excepto localhost `127.0.0.1`).

---

## **Componentes de una Dirección IP**
- **Dirección de Red**: Primera IP del rango (ej: `192.168.1.0/24`). **No asignable**.
- **Dirección de Broadcast**: Última IP del rango (ej: `192.168.1.255/24`). **No asignable**.
- **Hosts Válidos**: Todas las IPs entre red y broadcast (ej: `192.168.1.1` a `192.168.1.254`).

---

## **Tipos de Ejercicios**

### 1. **Identificar Clase y Parte de Red/Host**
**Ejemplo**: `150.10.15.0`  
- **Clase B** (primer octeto 150).  
- **Parte de red**: `150.10.0.0` (primeros 16 bits).  
- **Parte de host**: `0.0.15.0` (últimos 16 bits).  

---

### 2. **Calcular Subredes (Ej: 192.168.10.0/24 para 30 hosts)**
**Pasos**:
1. **Hosts necesarios**: `30 hosts` → `2^n ≥ 30+2` → `n=5` bits (32 direcciones, 30 usables).  
2. **Bits para subredes**: `8 (total) - 5 (hosts) = 3` → `2^3 = 8 subredes`.  
3. **Nueva máscara**: `/24 + 3 = /27` → `255.255.255.224`.  
4. **Rangos**:  
   - Subred 1: `192.168.10.0/27` → Hosts: `192.168.10.1` a `192.168.10.30`.  
   - Subred 2: `192.168.10.32/27` → Hosts: `192.168.10.33` a `192.168.10.62`.  
   - ... (saltos de 32 en el 4to octeto).  

---

### 3. **Determinar Rango de Hosts Válidos**
**Ejemplo**: `172.20.106.22/20`  
1. **Máscara**: `255.255.240.0` → Bloques de `16` en el 3er octeto.  
2. **Dirección de red**:
     Se calcula haciendo la puerta AND entre la IP y la Mascara de red
   - `172.20.106.22 AND 255.255.240.0` → `172.20.96.0`.  
3. **Broadcast**:
     Se calcula poniendo en 1 el tantos digitos como tiene de bits de numero de hosts
   - `172.20.111.255`.
4. **Hosts válidos**: `172.20.96.1` a `172.20.111.254`.  

---

### 4. **Verificar si una IP es Asignable**
**Ejemplo**: `201.45.116.159/27`  
1. **Máscara**: `255.255.255.224` → Bloques de `32` en el 4to octeto.  
2. **Subred**: `201.45.116.128` (últimos 5 bits en 0).  
3. **Broadcast**: `201.45.116.159` (últimos 5 bits en 1).  
4. **Rango válido**: `201.45.116.129` a `201.45.116.158`.  
   - **Conclusión**: `201.45.116.159` es **broadcast** → **No asignable**.  

---

## **Detalles Importantes**
- **VLSM (Subnetting Variable)**: Asignar máscaras diferentes según necesidades (ej: subredes de 200, 100, 50 hosts).  
- **Fórmulas clave**:  
  - Subredes: `2^n` (donde `n` = bits prestados).  
  - Hosts: `2^m - 2` (donde `m` = bits para hosts).  
- **Errores comunes**:  
  - Olvidar restar 2 (red y broadcast) al calcular hosts.  
  - Confundir dirección de red con la primera IP válida.  

---

## **Paso a Paso Detallado para Subredes**

### 1. **Identificar Clase y Máscara Inicial**
   - **Ejemplo**: Dirección `192.168.1.0/24` (Clase C).  
     - Máscara inicial: `255.255.255.0` (/24).  
     - **Total bits para hosts**: 8 (último octeto).  

---

### 2. **Calcular Bits Necesarios**
   - **Para hosts**:  
     - Si necesitas **30 hosts por subred**:  
       - Fórmula: `2^n ≥ 30 + 2` (red + broadcast).  
       - `n = 5` bits (porque `2^5 = 32` direcciones, 30 usables).  
   - **Para subredes**:  
     - Si necesitas **7 subredes**:  
       - Fórmula: `2^m ≥ 7` → `m = 3` bits (8 subredes posibles).  

---

### 3. **Ajustar Máscara**
   - **Bits prestados para subredes**: 3 (del octeto de hosts).  
   - **Nueva máscara**: `/24 + 3 = /27` → `255.255.255.224`.  
     - Binario: `11111111.11111111.11111111.11100000`.  

---

### 4. **Calcular Saltos de Subred**
   - **Fórmula**: Salto = `2^(bits de hosts restantes)`.  
     - En el ejemplo: `2^5 = 32` (porque usamos 3 bits para subredes y quedan 5 para hosts).  
   - **Saltos en el 4to octeto**:  
     - Subred 1: `192.168.1.0` (red) → Hosts: `192.168.1.1` a `192.168.1.30`.  
     - Subred 2: `192.168.1.32` (0 + 32) → Hosts: `192.168.1.33` a `192.168.1.62`.  
     - Subred 3: `192.168.1.64` (32 + 32) → Hosts: `192.168.1.65` a `192.168.1.94`.  
     - ...  
     - Subred 8: `192.168.1.224` → Hosts: `192.168.1.225` a `192.168.1.254`.  

   - **Patrón**: Los saltos son múltiplos de `32` en el último octeto.  

---

### 5. **Definir Rangos de Hosts**
   - **Primera IP válida**: Dirección de red + 1.  
     - Ej: Subred `192.168.1.32` → Primera IP: `192.168.1.33`.  
   - **Última IP válida**: Dirección de broadcast - 1.  
     - Ej: Broadcast de `192.168.1.63` → Última IP: `192.168.1.62`.  

---

## **Ejemplo Gráfico**
| Subred   | Dirección de Red | Rango de Hosts Válidos       | Broadcast      |
|----------|------------------|-----------------------------|----------------|
| 1        | 192.168.1.0      | 192.168.1.1 - 192.168.1.30  | 192.168.1.31   |
| 2        | 192.168.1.32     | 192.168.1.33 - 192.168.1.62 | 192.168.1.63   |
| ...      | ...              | ...                         | ...            |
| 8        | 192.168.1.224    | 192.168.1.225 - 192.168.1.254 | 192.168.1.255 |

---

## **Regla Clave para Saltos**
- **Máscara /X** → Saltos = `2^(8 - (X - 24))` (para Clase C).  
  - Ejemplo con `/27`:  
    - `27 - 24 = 3` bits para subredes → `2^3 = 8` subredes.  
    - Saltos: `2^(8 - 3) = 32` (en el último octeto).  
