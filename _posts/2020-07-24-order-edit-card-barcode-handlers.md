---
title: Добавлена обработка карт и штрихкодов на экране редактирования заказа
layout: default
---

Начиная с V7Preview4 плагин может обрабатывать события прокатки карты или сканирования штрихкода на экране редактирования заказа.
Это может быть использовано, например, для интеграции с внешней системой лояльности.

В `PluginContext.Notifications` добавлены два уведомления:

* [`OrderEditCardSlided`](https://iiko.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_OrderEditCardSlided.htm) — прокатка карты
* [`OrderEditBarcodeScanned`](https://iiko.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_OrderEditBarcodeScanned.htm) — сканирование штрихкода

Данные уведомления срабатывают лишь непосредственно на экране редактирования заказа (без открытых всплывающих окон) и только при прокатке карты или сканировании штрихкода, которые не были распознаны встроенными обработчиками.
Назначение скидки по зарегистрированной в iikoRms дисконтной карте, добавление в заказ товара по штрихкоду фасовки и прочие подобные операции работают как раньше, но если прокатанная карта или отсканированный штрихкод приложению iikoFront неизвестны, то наступает очередь плагина.
Зарегистрированный плагином обработчик получит штрихкод или [данные о карте](https://iiko.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_View_CardInputDialogResult.htm) и должен будет сообщить результат — считать ли уведомление обработанным.
Если плагин говорит, что событие обработано, на этом процесс завершается, обработчики других плагинов вызываться не будут.
Если плагин не знает, что это за штрихкод или карта, ему следует вернуть `false`, чтобы были вызваны обработчики других плагинов.
Если событие в итоге так и останется необработанным, пользователь получит стандартное сообщение о том, что прокатанная карта или отсканированный штрихкод системе неизвестны.

На время обработки уведомления на экране показывается прогрессбар.
Помимо штрихкода или данных о карте, плагин получит текущий заказ, локальную версию [`IOperationService`](https://iiko.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_IOperationService.htm) для редактирования текущего заказа, а также [`IViewManager`](https://iiko.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_UI_IViewManager.htm) с возможностью показывать диалоговые окна и [менять текст](https://iiko.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_UI_IViewManager_ChangeProgressBarMessage.htm) на прогрессбаре.

В SamplePlugin добавлены примеры использования: `OrderEditScreen.AddProductByBarcode` и `OrderEditScreen.AddDiscountByCard`.