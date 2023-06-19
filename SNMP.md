# Настройка SNMP

Здесь не будет полноценного ТЗ, так как лабораторная включала в себя много лишнего. Я просто опишу, как настраивать SNMP.

SNMP - протокол прикладного уровня, позволяющий управлять сетевым устройство - читать и писать разные данные (например посмотреть загрузку ЦПУ или поменять vlan на порту).

Есть три версии - 1, 2c, 3.

## Настройка
На устройстве нам нужно поднять snmp сервер который будет принимать запросы и отправлять SNMP-traps(события, о которых snmp-сервер уведомляет сам, без запроса).

Настройка community string и прав доступа (read-only или read-write)
```
(config)#snmp-server community COMMUNITY_STR ro | rw
```

Настройка с ограничением доступа через [ACL](ACL.md)
```
(config)#snmp-server community WORD ro | rw ACL_NAME | ACL_NUM
```

Указать получателя трапов
```
(config)#snmp-server host host-id COMMUNITY_STR
```

Указать получателя трапов и версии
```
(config)#snmp-server host host-id version (1 | 2c | 3 [auth | noauth | priv]) COMMUNITY_STR
```

Включение трапов
```
(config)#snmp-server enable traps
```

Включение определенных трапов
```
(config)#snmp-server enable traps bgp temperature
```

Проверка
```
(config)#show snmp
(config)#show snmp community
```

Рекомендации:
* SNMP может создавать уязвимости (для версий 1 и 2с узнав community кто угодно может читать, а может быть даже писать)
для версий 1 и 2с community строки должны быть надежными и часто меняться
* Нужно использовать ACL
* Лучше всего использовать SNMPv3 c аутентификацией и шифрованием

Настройка v3

Создание ACL
```
(config)#ip access-list standard ACL_NAME
(config-std-nacl)#permit A.B.C.D
```

Настройка представления SNMP
```
(config)#snmp-server view VIEW_NAME IOD_TREE
```

Настройка группы SNMP
```
(config)#snmp-server group GROUP_NAME v3 priv read VIEW_NAME access [ACL_NAME | ACL_NUM]
```

Настройка пользователя в качестве участника группы SNMP
```
(config)#snmp-server user USERNAME GROUP_NAME v3 auth (md5 | sha) PASSWORD priv (des | 3des | aes (128 | 192 | 256)) PASSWORD
```