## Tarefa 3. Host Virtuais. Autorización e Autenticación.

Necesitaremos ter un nome  DNS. 

Configuramos un servidor virtual para www.omeusitio.lan. Descomprime o arquivo omeusitio.lan.tar.gz en  e fai que se monte en /opt/web/oumeusitio.lan

A configuración do host virtual é a seguinte:

1. O directorio raíz de documentos debe ser /opt/web/omeusitio.lan/htdocs, e debe permitir o uso de ficheiros .htaccess relacionados coa autenticación e información de ficheiros. Tamén queremos habilitar a opción para amosar un listado do contido do directorio no caso de se introduza un directorio na URL e que non exista ningún dos ficheiros especificados como de procura nese caso. Indica cal é a configuración do sitio virtual e explícaa detalladamente.

    Ficheiro de configuración de apache:

    ![images](images/1_1.png)

2. No directorio imaxes, queremos permitir so que se descarguen os ficheiros con extensións .jpg, jpeg, bmp, gif, png e tiff. Todos os demais deberían esta bloqueados.  Explica o resultado obtido unha vez accedido cun navegador web á URL http://www.omeusitio.lan/imaxes tendo en conta o contido do directorio imaxes. Comproba se se pode acceder explicitamente ao ficheiro .pdf que hai dentro dese directorio. 

    Ficheiro de configuración de apache:

    ![images](images/2_1.png)

    Ao acceder a http://www.omeusitio.lan/imaxes non mostrará o contido del, e decir, o listado de ficheiros. Esto debese a que restrinximos o accesso a el con un "Require all denied"

    ![images](images/2_2.png)

    Ao acceder explicitamente ao ficheiro ".pdf" non mostrará o ficheiro. Esto debese a que restrinximos o accesso aos tipos de ficheiros que non estan indicados na directiva "<FilesMatch\>"

    ![images](images/2_3.png)


3. No directorio datos queremos bloquearlle o acceso aos clientes que teñan enderezo na rede 192.168.58.128/25 mediante ficheiros .htaccess. Indica o contido dese ficheiro, e tamén comproba con un cliente dentro desa subrede e outro fora se se pode acceder ou non a ese directorio.

    Ficheiro de configuración de apache:

    ![images](images/3_1.png)

    Un cliente que ten unha ip dentro de esa subred, non pode acceder:

    ![images](images/3_2.png)

    Un cliente que non ten esa ip dentro de esa subred, si pode acceder:

    ![images](images/3_3.png)

 
4. No directorio traballadores, queremos habilitar a autenticación Basic, e para elo crearemos os usuarios ana e eva. Gardaremos a lista de usuarios nun ficheiro en /opt/web/omeusitio.lan/ que debería ter un nome que impida ser accedido accidentalmente. Amosa a configuración, a ventá de acceso no navegador e a liña do log onde se ve o nome do usuario que fixo login.

    Ficheiro de configuración de apache:

    ![images](images/4_1.png)

    Lista de usuarios:

    ![images](images/4_2.png)

    Resultado de acceder a /traballadores:

    ![images](images/4_3.png)

    ![images](images/4_4.png)

5. No directorio directivos, queremos habilitar a autenticación Digest, e para elo crearemos os usuarios xan e lois. Gardaremos a lista de usuarios nun ficheiro en /opt/web/omeusitio.lan/ que debería ter un nome que impida ser accedido accidentalmente. Escolleremos como nome de dominio (realm) “directivos”. Amosa a configuración, a ventá de acceso no navegador e a liña do log onde se ve o nome do usuario que fixo login.

    Ficheiro de configuración de apache:

    ![images](images/5_1.png)

    Lista de usuarios:

    ![images](images/5_2.png)

    Resultado de acceder a /directivos:
    
    ![images](images/5_3.png)
    
    ![images](images/5_4.png)

6. No directorio contas, usando ficheiros .htaccess, queremos permitir o acceso a certos grupos de usuarios usando autenticación Basic. Gardaremos a lista de usuarios nun ficheiro en /opt/web/omeusitio.lan/ que debería ter un nome que impida ser accedido accidentalmente. Poderán acceder o usuario manolo e os membros do grupo directivos (matias, anton) que sexan a súa vez membros do grupo accionistas (anton, maría, olga) e que non sexan a súa vez membros do grupo temporais (matias, olga, xaime)

    Ficheiro de configuración ".htaccess":

    ![images](images/6_1.png)

    Lista de grupos:

    ![images](images/6_2.png)

    Lista de usuarios:

    ![images](images/6_3.png)

    Resultado de acceder a /contas:

    ![images](images/6_4.png)

    ![images](images/6_5.png)

    Probas con manolo, anton e matias. Manolo e anton poden acceder, matias non:

    ![images](images/6_6.png)   


7. Indica como se faría para impedirlle o acceso á URL http://www.omeusitio.lan/secure aos clientes que teñan un enderezo IP que comece por 172.16 e aos que teñan un nome cuxo dominio sexa apache.lan

    Unha forma:
    ```
    <Directory /opt/web/omeusitio.lan/htdocs/secure>
        <RequireAll>
            Require all granted
            <RequireNone>
                Require ip 172.16.0.0/16
                Require host apache.lan
            </RequireNone>
        </RequireAll>
    </Directory>
    ```

    Outra forma
    ```
    <Directory /opt/web/omeusitio.lan/htdocs/secure>
        <RequireAll>
            Require all granted
            Require not ip 172.16.0.0/16
            Require not host apache.lan
        </RequireAll>
    </Directory>
    ```

8. Indica como farías para configurar o acceso ao directorio reservado da seguinte maneira: Para poder acceder, ten que cumprirse que o usuario sexa manolo, ou pertencer ao grupo admin e máis administradores e a súa vez estar no grupo vendas ou comerciais, e que baixo ningún concepto se pertenza ao grupo temporais ou interinos ou que o enderezo IP sexa 192.168.58.99

    Un exemplo de como se faría:

    ```
    <Directory /opt/web/omeusitio.lan/htdocs/reservado>
        <RequireAny>
            Require user manolo
            <RequireAll>
                Require group admin
                Require group administradores
                <RequireAny>
                    Require group vendas
                    Require group comerciais
                </RequireAny>
                <RequireNone>
                    Require group temporais
                    Require group iterinos
                    Require ip 192.168.58.99
                </RequireNone>
            </RequireAll>
        </RequireAny>
    </Directory>
    ```
