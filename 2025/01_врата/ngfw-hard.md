### CyberCamp 2025 challenge

#### Название: NGFW-хард

##### Категория: Medium (75 сайбов) 

Райтап от `pula`, которая заняла 2-е место по результатам соло-участия!

Задание

<img width="685" height="453" alt="image_2025-11-01_15-54-16" src="https://github.com/user-attachments/assets/e74de6c8-4699-44b2-aa13-d0100ccc1101" />

Для начала заходим в правила для WAN. Видим, что входящие соединения по 22 порту (SSH) разрешены для любого источника, а это значит, что Василий сможет подключиться удаленно.
В ответе необходимо указать строку из столбца `Description`. Чтобы не переписывать все символы вручную, необходимо на главном экране `pfsense` нажать "Browse" и перейти к файлу `/cf/conf/config.xml`.
Через `ctrl+F` найти строку, которая начинается с первых символов `"dh;"` и скопировать все, что находится в квадратных скобках.

<img width="1609" height="927" alt="image_2025-11-01_16-04-40" src="https://github.com/user-attachments/assets/1cbb8f8e-0a0d-4ce3-a8b2-375d4de88e6c" />

Вставляем эту строку в `CyberChef`, выбираем `"From HTML Entity"` и получаем наш ответ. В последующих заданиях процесс аналогичен.

<img width="1074" height="602" alt="image_2025-11-01_16-08-26" src="https://github.com/user-attachments/assets/460792b2-b371-429e-b0b8-53ac65380bfc" />

Вопрос 2

<img width="689" height="416" alt="image_2025-11-01_16-10-05" src="https://github.com/user-attachments/assets/a5116c4b-ae24-45c8-b3ec-5790ac264e8f" />

В этом задании, находясь в файле `/cf/conf/config.xml`, вбиваем в поиск "group" и смотрим какие группы прав существуют.
Видим группу `"mygroup"`, она нам и нужна. Ответ берем из строки `<description>`, прогоняя ее через `CyberChef.`

<img width="1548" height="936" alt="image_2025-11-01_16-21-31" src="https://github.com/user-attachments/assets/7ff330da-e42d-4658-9f6d-a4a30ee031ea" />

Вопрос 3

<img width="693" height="515" alt="image_2025-11-01_16-22-16" src="https://github.com/user-attachments/assets/26bada6f-5bf5-4792-a5e7-566c88de0630" />

В файле `/cf/conf/config.xml` вбиваем в поиск `"3389"` (порт для `RDP`) и находим 3 правила. Сравнивая их видим, что в одном из правил отсутствует строка `"<log></log>"`.

Первый мэтч по `3389`

<img width="621" height="323" alt="image_2025-11-01_16-34-17" src="https://github.com/user-attachments/assets/b19dec36-1136-4f03-927e-6118cd35adaa" />

Второй мэтч по `3389`

<img width="621" height="217" alt="image_2025-11-01_16-35-11" src="https://github.com/user-attachments/assets/e91a795e-839d-43a3-8422-f3c560e09aa7" />

Третий мэтч по `3389`

<img width="623" height="251" alt="image_2025-11-01_16-35-54" src="https://github.com/user-attachments/assets/b9312a2e-337c-41b0-82ce-a936218b407e" />

Это и есть нужное правило, тк без этой строки опция логирования отключена. Ответ берем из строки `<descr>`, вновь прогоняя ее через CyberChef.

<img width="684" height="425" alt="image_2025-11-01_16-36-57" src="https://github.com/user-attachments/assets/bd692d36-79ad-4e41-80bb-abc57d0622ab" />

В файле `/cf/conf/config.xml` вбиваем в поиск `"snmp"` и находим нужное правило.

<img width="472" height="505" alt="image_2025-11-01_16-44-24" src="https://github.com/user-attachments/assets/1dde559f-96e8-46f5-9ce8-310a07117cb1" />

Прошерстив открытые источники я поняла, что значение `"public"` в строке `<rocommunity>public</rocommunity>` является небезопасным.
Это значение устанавливается по умолчанию и злоумышленник может использовать его для получения доступа к данным `SNMP` агента. Поэтому рекомендуется устанавливать здесь уникальное значение. Таким образом ответ в данном задании - `public`.

<img width="677" height="413" alt="image_2025-11-01_16-52-41" src="https://github.com/user-attachments/assets/6be3a17e-fdb5-4a61-a790-b25551ce2f85" />

Это задание оказалось чуть сложнее предыдущих. Переходим на вкладку `VPN -> IPsec` и видим 5 туннелей. Сложность в том, что выглядят они все абсолютно одинаково...

<img width="1161" height="730" alt="image_2025-11-01_16-59-17" src="https://github.com/user-attachments/assets/762aba91-4533-4e99-a6be-a3328dd6b69b" />

Поэтому я решила посмотреть настройки в файле `config.xml`. Здесь я выполнила поиск по ip-адресу `Remote Gateway`. Ниже приведу скриншоты настроек 1 и 4 туннелей (настройки 2,3 и 5 аналогичны настройкам 1-го).

Первый туннель 

<img width="725" height="662" alt="image_2025-11-01_17-03-26" src="https://github.com/user-attachments/assets/4d063f76-3007-4fa1-815d-e1b731ef694f" />

Четвертый туннель

<img width="617" height="664" alt="image_2025-11-01_17-04-39" src="https://github.com/user-attachments/assets/38fde994-cb0a-4818-9439-03de31271bab" />

Здесь мы видим, что `<pre-shared-key>` у 4-го туннеля имеет значение `12345`.
`<pre-shared-key>` — это секретный ключ для аутентификации. Соответственно чем проще значение, тем легче его подобрать)
В настройках всех остальных туннелей это значение уникально, поэтому слабость есть у туннеля под номером 4. Ответ берем из строки `<descr`>, прогоняя ее через CyberChef.

<img width="683" height="303" alt="image_2025-11-01_17-11-23" src="https://github.com/user-attachments/assets/20e864f9-2ada-4b2c-bbb4-ecf8fcf4c4a2" />

Ответы

```
dh;E@%B^=d!>&a'k?:rQnSG"xfZU
0=}5D5{E>.s$V#mrGH`2lbU.q0S=
9u{%VBJ%*K).J(~Qr:]&*%hO^p$0
public
ymk3V0J(aZJ!WO)ve_F[Y9-!JI=*
```
