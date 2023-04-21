class Employee:
    _name: str
    _salary: float
    def __init__(self, name: str) -> None:
        self._name = name
        self._salary = 100.0
    def CalculateSalary(self) -> float:
        return self._salary
class Cook(Employee):
    _isWorking: bool
    def __init__(self, name: str) -> None:
        super().__init__(name)
        self._salary = 150.0
        self._isWorking = False
class Waiter(Employee):
    _tips: float
    def __init__(self, name) -> None:
        super().__init__(name)
        self._salary = 135.0
        self._tips = 0.0
    def CalculateSalary(self) -> float:
        return super().CalculateSalary()+self._tips
class Restorans:
    _employees: 'list[Employee]' # Pēdiņās liek: Sarakstus, vārdnīcas utt.
                                 # Tā pat pēdiņās liek klases kuras ir definētas vēlāk
                                 # Piemēram, ja klase izmanto citu klasi, un tā klase izmanto 
                                 # šo klasi. Tad ieliek pēdiņās, lai pateiktu Python, ka klase
                                 # ir definēta kaut kur vēlāk.
    _kitchen: 'Kitchen'
    def __init__(self, employees: 'list[Employee]') -> None:
        self._employees = employees
        self._kitchen = Kitchen()
        for employee in self._employees:
            if type(employee) == Cook:
                self._kitchen._cooks.append(employee)

    def MakeOrder(self, client: 'Customer', order: 'Order') -> 'list[Food]':
        # Pārbaudam vai pietiek nauda
        price = order.CalculatePrice()
        if (price > client._availableMoney):
            print("Oops, nepietiek nauda!")
            return
        client._availableMoney -= price
        # Šeit vajadzētu vēl piesaistīt viesmīli, bet to mēs nedarīsim

        # Sagatavo ēdienu
        orders = []
        for order_food in order._order:
            orders.append(self._kitchen.Cook(order_food))

        return orders
class WhiteGood:
    _name: str
class Ingredient:
    _name: str
    _cost: float
    _uses: WhiteGood
class Recipe:
    _ingredients: 'dict[Ingredient, int]' # sastāvdaļa: skaits
    _name: str
    _secondsToCook: int
class Order:
    _order: 'list[Recipe]'
    def __init__(self, recipes: 'list[Recipe]') -> None:
        self._order = recipes
    def CalculatePrice(self) -> float:
        totalPrice = 0.0
        for order in self._order:
            for ingredient, count in order._ingredients.items():
                totalPrice += ingredient._cost * count
        return totalPrice

class Customer:
    _availableMoney: float
class Kitchen: 
    _cooks: 'list[Cook]'
    _availableIngredients: 'dict[Ingredient: int]'
    _whiteGoods: 'list[WhiteGood]'

    def __init__(self) -> None:
        self._cooks = []
        self._availableIngredients = {} ###
        self._whiteGoods = []
    
    def Cook(self, recipe: Recipe) -> 'Food':
        # Izvēlas pirmo pieejamo pavāru
        cook = None
        for a_cook in self._cooks:
            if a_cook._isWorking is False:
                cook = a_cook
                break
        if cook is not None:
            cook._isWorking = True
            print(f"Jūs apkalpos pavārs {cook._name}!")
        else:
            print("Nav pieejami pavāri!")
       
       # Pārbauda vai ir sastāvdaļas
        for ingredient in recipe._ingredients:
            isIngredientAvailable = False
            for availableIngredient in self._availableIngredients.keys():
                if availableIngredient._name == ingredient._name:
                    isIngredientAvailable = True
                    break
            if isIngredientAvailable is False:
                print(f"Nav pieejams {ingredient._name}")
                raise Exception("Nevar pabeigt pasūtījumu!")

        # Ēdiena pagatavošana
        result = Food()
        result._ingredients = recipe._ingredients
        for ingredient in recipe._ingredients:
            for availableIngredient in self._availableIngredients.keys():
                    if availableIngredient._name == ingredient._name:
                        self._availableIngredients[availableIngredient] -= recipe._ingredients[ingredient]
        result._name = recipe._name
        return result
class Food:
    _ingredients: 'list[Ingredient]'
    _name: str

# Klašu pielietošana (darbības izpilde - klients var nopirkt ēdienu)
Janis = Waiter("Janis")
Emils = Cook("Emils")

restorans = Restorans([Janis, Emils])
udens = Ingredient()
udens._name = "Udens"
udens._cost = 0.1
kartupeli = Ingredient()
kartupeli._name = "Kartupelis"
kartupeli._cost = 0.2
buljons = Ingredient()
buljons._name = "Buljons"
buljons._cost = 0.1
restorans._kitchen._availableIngredients = {udens: 10, kartupeli: 100, buljons: 100}

recepte = Recipe()
recepte._ingredients = {udens: 2, kartupeli: 8, buljons: 10}
recepte._name = "Buljons"

client = Customer()
client._availableMoney = 100.0

order = Order([recepte])
print(order.CalculatePrice())

rezultats = restorans.MakeOrder(client, order)
print(f"Nopirka {rezultats[0]._name}! Klientam palika {client._availableMoney} naudiņas!")
print("Virtuvē palika: ")
for sastavdala in restorans._kitchen._availableIngredients:
    print(f"{sastavdala._name}: {restorans._kitchen._availableIngredients[sastavdala]}")
