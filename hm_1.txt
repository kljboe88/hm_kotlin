import kotlin.random.Random

typealias CharName = String

interface Attacker {
    val baseDamage: Int
    fun attack(target: CharName): Int
}

interface Spellcaster {
    var mana: Int
    fun castSpell(spellName: String): Boolean
}

abstract class Hero(val name: String, var hp: Int) {
    abstract fun useSpecialAbility()

    fun takeDamage(damage: Int) {
        hp -= damage
        println("$name получает $damage урона. Осталось HP: $hp")
    }
}

class Warrior(name: String, hp: Int) : Hero(name, hp), Attacker {
    override val baseDamage: Int = 20

    override fun attack(target: CharName): Int {
        val isCrit = Random.nextInt(100) < 20
        val damage = if (isCrit) baseDamage * 2 else baseDamage

        if (isCrit) {
            println("$name наносит КРИТИЧЕСКИЙ урон по $target: $damage!")
        } else {
            println("$name атакует $target на $damage урона.")
        }
        return damage
    }

    override fun useSpecialAbility() {
        println("$name активирует ЯРОСТЬ! Его атаки стали сильнее!")
    }
}

class Mage(name: String, hp: Int) : Hero(name, hp), Spellcaster {
    override var mana: Int = 100

    override fun castSpell(spellName: String): Boolean {
        if (mana > 20) {
            mana -= 20
            println("$name произносит заклинание \"$spellName\". Осталось маны: $mana")
            return true
        } else {
            println("$name: не хватает маны на \"$spellName\"!")
            return false
        }
    }

    override fun useSpecialAbility() {
        mana += 50
        println("$name восстанавливает 50 маны! Теперь маны: $mana")
    }
}

class Paladin(name: String, hp: Int) : Hero(name, hp), Attacker, Spellcaster {
    override val baseDamage: Int = 15
    override var mana: Int = 60

    override fun attack(target: CharName): Int {
        println("$name наносит священный урон по $target: $baseDamage")
        return baseDamage
    }

    override fun castSpell(spellName: String): Boolean {
        if (mana > 20) {
            mana -= 20
            println("$name творит \"$spellName\". Осталось маны: $mana")
            return true
        } else {
            println("$name: не хватает маны!")
            return false
        }
    }

    override fun useSpecialAbility() {
        if (castSpell("Священный свет")) {
            hp += 30
            println("$name исцелил себя на 30 HP! Теперь HP: $hp")
            attack("враг")
        } else {
            println("$name не удалось применить способность.")
        }
    }
}

fun main() {
    val warrior = Warrior("Гаррок", 100)
    val mage = Mage("Мерлин", 80)
    val paladin = Paladin("Артур", 120)

    warrior.attack("Гоблин")
    warrior.takeDamage(15)
    warrior.useSpecialAbility()

    mage.castSpell("Огненный шар")
    mage.useSpecialAbility()

    paladin.takeDamage(40)
    paladin.useSpecialAbility()
}