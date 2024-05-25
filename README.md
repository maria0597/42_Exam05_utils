# 42_Exam05_utils
# TRYtoLEARN by Warlock

Il gioco si svolge in un mondo magico dove il giocatore interpreta un mago che deve imparare e usare vari incantesimi per superare ostacoli e combattere nemici. 
Il mago deve viaggiare attraverso un regno infestato, imparare nuovi incantesimi da antichi tomi magici e usare questi incantesimi per progredire.

**!Qui di seguito, analizziamo la gerarchia di ereditarietà e le relazioni tra gli oggetti, insieme a una spiegazione dettagliata di come creare e collegare tutti gli oggetti!**

### Gerarchia di Ereditarietà

#### Classi di Base

1. **ASpell**
    
    - Classe astratta che rappresenta un incantesimo.
    - Attributi: `name`, `effects`.
    - Metodi:
        - `getName() const`
        - `getEffects() const`
        - `virtual ASpell* clone() const = 0` 
2. **ATarget**
    
    - Classe astratta che rappresenta un bersaglio.
    - Attributo: `type`.
    - Metodi:
        - `getType() const`
        - `virtual ATarget* clone() const = 0`
        - `void getHitBySpell(ASpell const &spell) const`

#### Classi Derivate

3. **Fwoosh** (derivata da ASpell)
    
    - Implementa un incantesimo specifico con nome "Fwoosh" e effetto "fwooshed".
    - Metodo:
        - `ASpell* clone() const override` (ritorna un nuovo oggetto Fwoosh)
4. **Fireball** (derivata da ASpell)
    
    - Implementa un incantesimo specifico con nome "Fireball" e effetto "burnt to a crisp".
    - Metodo:
        - `ASpell* clone() const override` (ritorna un nuovo oggetto Fireball)
5. **Polymorph** (derivata da ASpell)
    
    - Implementa un incantesimo specifico con nome "Polymorph" e effetto "turned into a critter".
    - Metodo:
        - `ASpell* clone() const override` (ritorna un nuovo oggetto Polymorph)
6. **BrickWall** (derivata da ATarget)
    
    - Implementa un bersaglio specifico con tipo "Inconspicuous Red-brick Wall".
    - Metodo:
        - `ATarget* clone() const override` (ritorna un nuovo oggetto BrickWall)
7. **Dummy** (derivata da ATarget)
    
    - Implementa un bersaglio specifico con tipo "Target Practice Dummy".
    - Metodo:
        - `ATarget* clone() const override` (ritorna un nuovo oggetto Dummy)

### Associazioni e Interazioni

1. **Warlock**
    
    - Contiene uno `SpellBook` che gestisce gli incantesimi conosciuti.
    - Metodi:
        - `learnSpell(ASpell* spell)`: Aggiunge un incantesimo al libro.
        - `forgetSpell(string const &name)`: Rimuove un incantesimo dal libro.
        - `launchSpell(string const &name, ATarget const &target)`: Lancia un incantesimo su un bersaglio.
2. **SpellBook**
    
    - Gestisce una collezione di incantesimi conosciuti (`std::vector<ASpell*>`).
    - Metodi:
        - `learnSpell(ASpell* spell)`: Copia un incantesimo nel libro.
        - `forgetSpell(string const &name)`: Elimina un incantesimo dal libro.
        - `ASpell* createSpell(string const &name)`: Crea e ritorna un incantesimo basato sul nome.
3. **TargetGenerator**
    
    - Gestisce una collezione di tipi di bersagli conosciuti (`std::vector<ATarget*>`).
    - Metodi:
        - `learnTargetType(ATarget* target)`: Aggiunge un tipo di bersaglio.
        - `forgetTargetType(string const &type)`: Rimuove un tipo di bersaglio.
        - `ATarget* createTarget(string const &type)`: Crea e ritorna un bersaglio basato sul tipo.
    
**GRAFO**
```mermaid
classDiagram
    class Warlock {
        -string name
        -string title
        -SpellBook spellBook
        +string const &getName()
        +string const &getTitle()
        +void setTitle(string const &)
        +void introduce() const
        +void learnSpell(ASpell*)
        +void forgetSpell(string const &)
        +void launchSpell(string const &, ATarget &)
    }

    class SpellBook {
        -vector<ASpell*> spells
        +void learnSpell(ASpell*)
        +void forgetSpell(string const &)
        +ASpell* createSpell(string const &)
    }

    class ASpell {
        -string name
        -string effects
        +string const &getName() const
        +string const &getEffects() const
        +ASpell* clone() const
        +void launch(ATarget &) const
    }

    class Fireball {
        -string name = "Fireball"
        -string effects = "burnt to a crisp"
        +Fireball* clone() const
    }

    class Polymorph {
        -string name = "Polymorph"
        -string effects = "turned into a critter"
        +Polymorph* clone() const
    }

    class Fwoosh {
        -string name = "Fwoosh"
        -string effects = "fwooshed"
        +Fwoosh* clone() const
    }

    class ATarget {
        -string type
        +string const &getType() const
        +ATarget* clone() const
        +void getHitBySpell(ASpell const &)
    }

    class Dummy {
        -string type = "Target Practice Dummy"
        +Dummy* clone() const
    }

    class BrickWall {
        -string type = "Inconspicuous Red-brick Wall"
        +BrickWall* clone() const
    }

    Warlock --> SpellBook
    SpellBook --> ASpell
    ASpell <|-- Fireball
    ASpell <|-- Polymorph
    ASpell <|-- Fwoosh
    ASpell --> ATarget : launch
    ATarget <|-- Dummy
    ATarget <|-- BrickWall
    ATarget --> ASpell : getHitBySpell
