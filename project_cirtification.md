**Build a User Configuration Manager**

test\_settings = {

&#x20;   'theme': 'light',

&#x20;   'notifications': 'enabled',

&#x20;   'volume': 'high'

}





def add\_setting(settings, setting):

&#x20;   key, value = setting

&#x20;   key = key.lower()

&#x20;   value = value.lower()



&#x20;   if key in settings:

&#x20;       return f"Setting '{key}' already exists! Cannot add a new setting with this name."



&#x20;   settings\[key] = value

&#x20;   return f"Setting '{key}' added with value '{value}' successfully!"





def update\_setting(settings, setting):

&#x20;   key, value = setting

&#x20;   key = key.lower()

&#x20;   value = value.lower()



&#x20;   if key in settings:

&#x20;       settings\[key] = value

&#x20;       return f"Setting '{key}' updated to '{value}' successfully!"



&#x20;   return f"Setting '{key}' does not exist! Cannot update a non-existing setting."





def delete\_setting(settings, key):

&#x20;   key = key.lower()



&#x20;   if key in settings:

&#x20;       del settings\[key]

&#x20;       return f"Setting '{key}' deleted successfully!"



&#x20;   return "Setting not found!"





def view\_settings(settings):

&#x20;   if not settings:

&#x20;       return "No settings available."



&#x20;   result = "Current User Settings:\\n"



&#x20;   for key, value in settings.items():

&#x20;       result += f"{key.capitalize()}: {value}\\n"



&#x20;   return result



**Build a Budget App**



class Category:

&#x20;   def \_\_init\_\_(self, name):

&#x20;       self.name = name

&#x20;       self.ledger = \[]



&#x20;   def deposit(self, amount, description=""):

&#x20;       self.ledger.append({"amount": amount, "description": description})



&#x20;   def withdraw(self, amount, description=""):

&#x20;       if self.check\_funds(amount):

&#x20;           self.ledger.append({"amount": -amount, "description": description})

&#x20;           return True

&#x20;       return False



&#x20;   def get\_balance(self):

&#x20;       return sum(item\["amount"] for item in self.ledger)



&#x20;   def transfer(self, amount, category):

&#x20;       if self.check\_funds(amount):

&#x20;           self.withdraw(amount, f"Transfer to {category.name}")

&#x20;           category.deposit(amount, f"Transfer from {self.name}")

&#x20;           return True

&#x20;       return False



&#x20;   def check\_funds(self, amount):

&#x20;       return amount <= self.get\_balance()



&#x20;   def \_\_str\_\_(self):

&#x20;       title = self.name.center(30, "\*")

&#x20;       lines = \[title]

&#x20;       for item in self.ledger:

&#x20;           desc = item\["description"]\[:23].ljust(23)

&#x20;           amt = f"{item\['amount']:.2f}".rjust(7)

&#x20;           lines.append(f"{desc}{amt}")

&#x20;       lines.append(f"Total: {self.get\_balance():.2f}")

&#x20;       return "\\n".join(lines)





def create\_spend\_chart(categories):

&#x20;   # 1. Calculate spent amounts (withdrawals only, excluding transfers out if any, though standard test cases treat withdrawal amounts directly)

&#x20;   withdrawals = \[]

&#x20;   for cat in categories:

&#x20;       spent = sum(-item\["amount"] for item in cat.ledger if item\["amount"] < 0)

&#x20;       withdrawals.append(spent)



&#x20;   total\_spent = sum(withdrawals)



&#x20;   # 2. Calculate percentages (rounded down to nearest 10)

&#x20;   percentages = \[]

&#x20;   for spent in withdrawals:

&#x20;       if total\_spent > 0:

&#x20;           pct = int((spent / total\_spent) \* 100) // 10 \* 10

&#x20;       else:

&#x20;           pct = 0

&#x20;       percentages.append(pct)



&#x20;   chart = "Percentage spent by category\\n"



&#x20;   # 3. Y-axis and bars (100 down to 0)

&#x20;   for level in range(100, -1, -10):

&#x20;       chart += str(level).rjust(3) + "| "

&#x20;       for pct in percentages:

&#x20;           chart += "o  " if pct >= level else "   "

&#x20;       chart += "\\n"



&#x20;   # 4. Horizontal line

&#x20;   chart += "    " + "-" \* (len(categories) \* 3 + 1) + "\\n"



&#x20;   # 5. Vertical category names

&#x20;   names = \[cat.name for cat in categories]

&#x20;   max\_len = max(len(name) for name in names)



&#x20;   for i in range(max\_len):

&#x20;       chart += "     "  # 5 spaces padding to align under first letter column

&#x20;       for j, name in enumerate(names):

&#x20;           char = name\[i] if i < len(name) else " "

&#x20;           chart += char + "  "  # Character followed by 2 spaces

&#x20;       if i < max\_len - 1:

&#x20;           chart += "\\n"



&#x20;   return chart

**Build a Polygon Area Calculator**



class Rectangle:

&#x20;   def \_\_init\_\_(self, width, height):

&#x20;       self.width = width

&#x20;       self.height = height



&#x20;   def set\_width(self, width):

&#x20;       self.width = width



&#x20;   def set\_height(self, height):

&#x20;       self.height = height



&#x20;   def get\_area(self):

&#x20;       return self.width \* self.height



&#x20;   def get\_perimeter(self):

&#x20;       return 2 \* self.width + 2 \* self.height



&#x20;   def get\_diagonal(self):

&#x20;       return (self.width \*\* 2 + self.height \*\* 2) \*\* 0.5



&#x20;   def get\_picture(self):

&#x20;       if self.width > 50 or self.height > 50:

&#x20;           return "Too big for picture."

&#x20;       return ("\*" \* self.width + "\\n") \* self.height



&#x20;   def get\_amount\_inside(self, shape):

&#x20;       return (self.width // shape.width) \* (self.height // shape.height)



&#x20;   def \_\_str\_\_(self):

&#x20;       return f"Rectangle(width={self.width}, height={self.height})"





class Square(Rectangle):

&#x20;   def \_\_init\_\_(self, side):

&#x20;       super().\_\_init\_\_(side, side)



&#x20;   def set\_side(self, side):

&#x20;       self.width = side

&#x20;       self.height = side



&#x20;   def set\_width(self, width):

&#x20;       self.set\_side(width)



&#x20;   def set\_height(self, height):

&#x20;       self.set\_side(height)



&#x20;   def \_\_str\_\_(self):

&#x20;       return f"Square(side={self.width})"



**Build a Hash Table**



class HashTable:

&#x20;   def \_\_init\_\_(self):

&#x20;       self.collection = {}



&#x20;   def hash(self, key):

&#x20;       return sum(ord(char) for char in key)



&#x20;   def add(self, key, value):

&#x20;       hashed = self.hash(key)

&#x20;       if hashed not in self.collection:

&#x20;           self.collection\[hashed] = {}

&#x20;       self.collection\[hashed]\[key] = value



&#x20;   def remove(self, key):

&#x20;       hashed = self.hash(key)

&#x20;       if hashed in self.collection and key in self.collection\[hashed]:

&#x20;           del self.collection\[hashed]\[key]

&#x20;           if not self.collection\[hashed]:

&#x20;               del self.collection\[hashed]



&#x20;   def lookup(self, key):

&#x20;       hashed = self.hash(key)

&#x20;       if hashed in self.collection:

&#x20;           return self.collection\[hashed].get(key)

&#x20;       return None



**Implement the Tower of Hanoi Algorithm**



def hanoi\_solver(n):

&#x20;   rods = \[list(range(n, 0, -1)), \[], \[]]

&#x20;   states = \[' '.join(str(rod) for rod in rods)]



&#x20;   def move(count, source, target, auxiliary):

&#x20;       if count == 0:

&#x20;           return

&#x20;       move(count - 1, source, auxiliary, target)

&#x20;       rods\[target].append(rods\[source].pop())

&#x20;       states.append(' '.join(str(rod) for rod in rods))

&#x20;       move(count - 1, auxiliary, target, source)



&#x20;   move(n, 0, 2, 1)

&#x20;   return '\\n'.join(states)



