# Hendrix
Hendrix представляет собой приватный мессенджер, написанный с использованием фреймворка Electron. Полгода назад меня посетила мысль создать свой собственный мессенджер, совместить его с возможностями криптографии и сделать таким образом его приватным. Спустя 2 месяца работы у меня что-то получилось, и теперь вы видите это. Моя разработка ни на что не претендует, она просто есть и всё. Я хотел создать её и создал, основываясь на своём скромном опыте, который постепенно растёт, расту я, и качество моих программ. Надеюсь на это.
# Как примерно это работает?
В самом начале разработки я задумался о том, как мне передать сообщение от Алисы к Бобу, когда они находятся на разных концах планеты и имеют доступ к глобальной сети. Очевидно, нам нужен сервер, который тоже будет подключен к глобальной сети и будет выступать в качестве посредника между Алисой и Бобом. Так я и сделал. Сейчас я арендую один сервер в Москве, которого пока хватает для коммуникации меня и моих друзей.
# Что подразумевается под приватностью?
Это означает, что третьи лица, прослушивая канал связи и перехватывая все пакеты, никогда не узнают, какая информация была передана. Поскольку у меня нет сертификата (покупать его нецелесообразно, я считаю), все пакеты передаются по обычному http, который легко "подслушать". Приватность обеспечивается совокупностью ассиметричной и симметричной криптографии. Опишу алгоритм сопряжения пользователя с сервером, конечной целью которого является синхронизация симметричного ключа:
1. Сгенерировать открытый и закрытый ключи криптосистемы RSA. Длина ключа составляет 2048 бит. Криптосистема RSA и поможет нам безопасно передать симметричный ключ. Далее открытый ключ отправляется на сервер.
2. Сервер понимает, что ему прислали открытый ключ и начинает генерацию симметричного ключа. Для генерации используется криптостойкий генератор псевдослучайных чисел, что также поможет избежать компрометации ключа. В качестве симметричного алгоритма шифрования используется 256-битный AES. Сгенерированный ключ шифруется открытым ключом криптосистемы RSA и отправляется в ответе на запрос пользователя и запоминается сервером, так как далее пользователь будет шифровать свои пакеты этим ключом, и серверу нужно их расшифровывать.
3. 