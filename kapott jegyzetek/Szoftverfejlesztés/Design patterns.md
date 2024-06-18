# Design patterns

Tervezési minták. Mindegy mit csinálsz, mindegy milyen nyelven dolgozol, mindenhol hasznosak lehetnek. A tervezési minták jól bevált technikák, amiket szoftverfejlesztés során használhatunk. Nem is magyarázom tovább, jöhetnek a minták.

## Builder pattern

Van egy hatalmas osztályod tele adatokkal? Túl sok paraméter, amiket meg kell adogatni, hogy létrehozz egy objektumot? A Builder minta segít ezen. Az osztályodnak lesz egy belső (Builder) osztálya, ami létrehozza az objektumot.

Példa Kotlin nyelvben:

```kt
class Notification {
    var title: String? = null
    var messages: List<String> = emptyList()
    var icon: String? = null
    var color: String? = null
    var sound: String? = null
    var vibration: Boolean = false

    class Builder {
        private val notification = Notification()

        fun setTitle(title: String) = apply { notification.title = title }
        fun setMessages(messages: List<String>) = apply { notification.messages = messages }
        fun addMessage(message: String) = apply { notification.messages += message }
        fun setIcon(icon: String) = apply { notification.icon = icon }
        fun setColor(color: String) = apply { notification.color = color }
        fun setSound(sound: String) = apply { notification.sound = sound }
        fun setVibration(vibration: Boolean) = apply { notification.vibration = vibration }

        fun build() = notification
    }
}
```

Mire jó ez a sok kód? Hát hogy ez helyett:

```kt
val notif = Notification("Title", listOf("Message1", "Message2"), "Icon", "Color", "Sound", true)
```

Ezt írhasd:

```kt
val notifBuilder = Notification.Builder()
    .setTitle("Title")
    .setIcon("Icon")
    .setColor("Color")
    .setSound("Sound")
    .setVibration(true)
    .build()

messages.forEach { notifBuilder.addMessage(it) }

val notif = notifBuilder.build()
```

Átláthatóbb, könnyebb kihagyni a nem kötelező paramétereket, bármilyen sorrendben megadhatod az adatokat és akár szét is választhatod egy objektum létrehozását több helyre.

### Dependency injection

Ijesztő neve van, de nagyon egyszerű. A lényege, hogy a kódod egy függőséget kívülről kap meg.

Példa Kotlin nyelvben:

```kt
