---
title: Возможность назначить официанта, обслуживающего заказ
layout: default
---
Начиная с V6 появится возможность указать официанта при создании заказа, а также сменить официанта позднее.

В версиях до V5 включительно пользователь, от имени которого действует плагин, автоматически назначался официантом всех создаваемых плагином заказов, и в дальнейшем сменить официанта через API было нельзя. Начиная с V6 у метода `CreateOrder` появляется необязательный аргумент, позволяющий указать другого официанта (по умолчанию официантом, как и прежде, будет текущий пользователь), а также добавляется метод `ChangeOrderWaiter`, через который можно назначить нового официанта.