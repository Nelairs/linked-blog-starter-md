Se observan intentos incesantes de explotar la vulnerabilidad RCE sin autenticacion en los equipos ZyXEL. Están asociados al UserAgent "Hello, world", "r00ts3c-owned-you" o "MtmKilledYou", todos ellos conectados a la red de bots Mirai.

Este ataque busca la explotacion de zhttp server de Zyxel en sus equipos.

## **IPs PAE involucradas alertas EDR:**

|IP|Servicio|Ocurrencia|
|---|---|---|
|200.4.59.236|AxionCard|4|
|200.4.59.181|Sitio institucional|4|

## **ID Alertas EDR Trend Micro:**

WB-14396-20230831-00001

WB-14396-20230831-00002

WB-14396-20230831-00003

WB-14396-20230831-00004

  

  

  

## Impacto

Sistema comprometido: Los atacantes pueden ganar control de los sistemas vulnerables.

  

---

El script fue encontrado en un repositorio del user R00tS3c, el cual brinda un scanner referido a Zyxel.

Del mismo se puede resaltar la función que crea IPs random. El mismo por default itera en segmentos con el primer octeto hardcodeado siendo uno de ellos el 200.X.X.X. Esto hace que itere en un momento por el segmento de IPs de PAE 200.4.59.X

- FRAGMENTO DE CODIGO
    
    ```C
    static ipv4_t zyxelscanner_get_random_ip(void)
    {
        uint32_t tmp;
        uint8_t o1 = 0, o2 = 0, o3 = 0, o4 = 0;
    
        do
        {
            tmp = rand_next();
    
            o1 = tmp & 0xff;
            o2 = (tmp >> 8) & 0xff;
            o3 = (tmp >> 16) & 0xff;
            o4 = (tmp >> 24) & 0xff;
        }
        while(o1 == 127 ||                             // 127.0.0.0/8      - Loopback
              (o1 == 0) ||                              // 0.0.0.0/8        - Invalid address space
              (o1 == 3) ||                              // 3.0.0.0/8        - General Electric Company
              (o1 == 15 || o1 == 16) ||                 // 15.0.0.0/7       - Hewlett-Packard Company
              (o1 == 56) ||                             // 56.0.0.0/8       - US Postal Service
              (o1 == 10) ||                             // 10.0.0.0/8       - Internal network
              (o1 == 192 && o2 == 168) ||               // 192.168.0.0/16   - Internal network
              (o1 == 172 && o2 >= 16 && o2 < 32) ||     // 172.16.0.0/14    - Internal network
              (o1 == 100 && o2 >= 64 && o2 < 127) ||    // 100.64.0.0/10    - IANA NAT reserved
              (o1 == 169 && o2 > 254) ||                // 169.254.0.0/16   - IANA NAT reserved
              (o1 == 198 && o2 >= 18 && o2 < 20) ||     // 198.18.0.0/15    - IANA Special use
              (o1 >= 224) ||                            // 224.*.*.*+       - Multicast
              (o1 == 6 || o1 == 7 || o1 == 11 || o1 == 21 || o1 == 22 || o1 == 26 || o1 == 28 || o1 == 29 || o1 == 30 || o1 == 33 || o1 == 55 || o1 == 214 || o1 == 215) // Department of Defense
        );
    
        int randnum = rand() % 10;
        if (randnum == 0)
        {
            return INET_ADDR(169,o2,o3,o4);
        }
        if (randnum == 1)
        {
            return INET_ADDR(80,o2,o3,o4);
        }
    	if (randnum == 2)
        {
            return INET_ADDR(86,o2,o3,o4);
        }
        if (randnum == 3)
        {
            return INET_ADDR(178,o2,o3,o4);
        }
    	if (randnum == 4)
        {
            return INET_ADDR(213,o2,o3,o4);
        }
    	if (randnum == 5)
        {
            return INET_ADDR(83,o2,o3,o4);
        }
        if (randnum == 6)
        {
            return INET_ADDR(200,o2,o3,o4);
        }
        if (randnum == 7)
        {
            return INET_ADDR(181,o2,o3,o4);
        }
        if (randnum == 8)
        {
            return INET_ADDR(82,o2,o3,o4);
        }
        if (randnum == 9)
        {
            return INET_ADDR(206,o2,o3,o4);
        }
        if (randnum == 10)
        {
            return INET_ADDR(217,o2,o3,o4);
        }
    }
    \#endif
    ```
    

  

## IoCs

![[Untitled 543.png|Untitled 543.png]]

IPs encontradas que alojan el binario:

|   |   |   |
|---|---|---|
|**IP ADDRESS**|**COUNTRY**|**AS**|
|**1.233.206[.]27**|**KR**|**AS 4665 ( Yonsei University )**|
|**210.221.206[.]138**|**KR**|**AS 3786 ( LG DACOM Corporation )**|
|**218.145.61[.]20**|**KR**|**AS 4766 ( Korea Telecom )**|
|**175.196.233[.]99**|**KR**|**AS 4766 ( Korea Telecom )**|
|**117.30.39[.]183**|**CN**|**AS 4134 ( Chinanet )**|
|**27.154.111[.]21**|**CN**|**AS 4134 ( Chinanet )**|
|**117.30.38[.]204**|**CN**|**AS 4134 ( Chinanet )**|
|**39.108.138[.]206**|**CN**|**AS 37963 ( Hangzhou Alibaba Advertising Co.,Ltd. )**|
|**121.41.84[.]196**|**CN**|**AS 37963 ( Hangzhou Alibaba Advertising Co.,Ltd. )**|
|**94.229.79[.]10**|**GB**|**AS 42831 ( UK Dedicated Servers Limited )**|
|**138.68.97[.]26**|**DE**|**AS 14061 ( DIGITALOCEAN-ASN)**|
|**196.196.196[.]3**|**IE**|**AS 58065 ( Packet Exchange Limited )**|
|**77.150.235[.]197**|**FR**|**AS 15557 ( Societe Francaise Du Radiotelephone – SFR SA)**|
|**196.196.196[.]2**|**IE**|**AS 58065 ( Packet Exchange Limited )**|
|218.92.247[.]138|CN|AS 4134  <br>  <br>( Chinanet )|

  

  

## Conclusión  
  

En conclusión esta actividad se detectó desde hace más de 30 días atrás, en cuanto al impacto para PAE es mínimo, ya que no se utiliza tecnología de Zyxel.

![[Untitled 1 9.png|Untitled 1 9.png]]

  
Para resaltar se encontraron IPs de PAE las cuales no tenemos en la lista enviada por el equipo.  
  
  

|Ips públicas PAE afectadas desconocidas|
|---|
|200.4.59.21|
|200.4.59.34|
|200.4.59.221|
|200.4.59.51|
|200.4.59.99|
|200.4.59.160|
|200.4.59.20|
|200.4.59.33|
|200.4.59.195|
|200.4.59.49|
|200.4.59.115|
|200.4.59.196|
|200.4.59.198|

De estas IPs se desconoce el servicio por detrás, esto sería bueno a nivel de información de apoyo en investigaciones.