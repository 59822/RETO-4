# RETO-4

## Shape:

``` python
import math

class Point:
    def __init__(self, x: int, y: int):
        self._x = x
        self._y = y

    def get_x(self):
        return self._x

    def set_x(self, value):
        self._x = value

    def get_y(self):
        return self._y

    def set_y(self, value):
        self._y = value

    def compute_distance(self, other_point: 'Point'):
        return ((other_point.get_x() - self.get_x()) ** 2 + (other_point.get_y() - self.get_y()) ** 2) ** 0.5


class Line:
    def __init__(self, start_point: Point, end_point: Point):
        self.start_point = start_point
        self.end_point = end_point
    
    def length(self):
        calcu = self.start_point.compute_distance(self.end_point)
        return calcu

class Shape:
    is_Regular = True

    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        self.vertices = vertices
        self.edges = edges
        self.inner_angles = inner_angles

    def compute_area(self):
        pass
    
    def compute_perimeter(self):
        return sum(edge.length() for edge in self.edges)

    def compute_inner_angles(self):
        count = len(self.vertices)
        return (count-2) * 180
        

class Rectangle(Shape):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 4:
            raise ValueError("A rectangle must have 4 vertices")

    def compute_area(self):
        length1 = self.edges[0].length()
        length2 = self.edges[1].length()
        return length1 * length2

class Square(Rectangle):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)       
        if len(vertices) != 4:
            raise ValueError("A Square must have 3 vertices")
        if self.edges[0].length() != self.edges[1].length():
            raise ValueError("A square must have equal sides")


class Triangle(Shape):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 3:
            raise ValueError("A triangle must have 3 vertices")
    def area_triangle(self):
        s=0.5*self.compute_perimeter()
        return math.sqrt(s*(s-self.edges[0].length())*(s-self.edges[1].length())*(s-self.edges[2].length()))
    
class EquilateralTriangle(Triangle):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 3:
            raise ValueError("A triangle must have 3 vertices")
        if not (self.edges[0].length() == self.edges[1].length() == self.edges[2].length()):
            raise ValueError("An Equilateral triangle must have three equal sides")
        
class IsoscelesTriangle(Triangle):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 3:
            raise ValueError("A triangle must have 3 vertices")
        if not ((self.edges[0].length() == self.edges[1].length() != self.edges[2].length()) or
                (self.edges[0].length() == self.edges[2].length() != self.edges[1].length()) or
                (self.edges[1].length() == self.edges[2].length() != self.edges[0].length())):
            raise ValueError("An Isosceles triangle must have two equal sides")
        
class ScalenTriangle(Triangle):
    def __init__(self, vertices: list[Point], edges: list[Line], inner_angles: list[float]):
        super().__init__(vertices, edges, inner_angles)
        if len(vertices) != 3:
            raise ValueError("A triangle must have 3 vertices")
        if not ((self.edges[0].length() != self.edges[1].length() != self.edges[2].length())):
            raise ValueError("A Scalene triangle must have three different sides")

vertex1 = Point(0, 0)
vertex2 = Point(0, 1)
vertex3 = Point(1, 1)
vertex4 = Point(1, 0)

line1 = Line(vertex1, vertex2)
line2 = Line(vertex2, vertex3)
line3 = Line(vertex3, vertex4)
line4 = Line(vertex4, vertex1)

rectangle = Square([vertex1, vertex2, vertex3, vertex4], [line1, line2, line3, line4], [90, 90, 90, 90])

print("Perímetro del rectángulo:", rectangle.compute_perimeter())

print("Área del rectángulo:", rectangle.compute_area())
print(rectangle.compute_inner_angles())

triangle_vertex1 = Point(0, 0)
triangle_vertex2 = Point(3, 0)
triangle_vertex3 = Point(1.5, 3 * math.sqrt(3) / 2)  

triangle_line1 = Line(triangle_vertex1, triangle_vertex2)
triangle_line2 = Line(triangle_vertex2, triangle_vertex3)
triangle_line3 = Line(triangle_vertex3, triangle_vertex1)

equilateral_triangle = EquilateralTriangle([triangle_vertex1, triangle_vertex2, triangle_vertex3],[triangle_line1, triangle_line2, triangle_line3], [60, 60, 60])

isosceles_vertex1 = Point(0, 0)
isosceles_vertex2 = Point(3, 0)
isosceles_vertex3 = Point(1.5, 2)  

isosceles_line1 = Line(isosceles_vertex1, isosceles_vertex2)
isosceles_line2 = Line(isosceles_vertex2, isosceles_vertex3)
isosceles_line3 = Line(isosceles_vertex3, isosceles_vertex1)

isosceles_triangle = IsoscelesTriangle([isosceles_vertex1, isosceles_vertex2, isosceles_vertex3], [isosceles_line1, isosceles_line2, isosceles_line3], [60, 60, 60])

scalene_vertex1 = Point(0, 0)
scalene_vertex2 = Point(3, 0)
scalene_vertex3 = Point(2, 4)  

scalene_line1 = Line(scalene_vertex1, scalene_vertex2)
scalene_line2 = Line(scalene_vertex2, scalene_vertex3)
scalene_line3 = Line(scalene_vertex3, scalene_vertex1)

scalene_triangle = ScalenTriangle([scalene_vertex1, scalene_vertex2, scalene_vertex3],[scalene_line1, scalene_line2, scalene_line3], [60, 60, 60])

square_vertex1 = Point(0, 0)
square_vertex2 = Point(0, 1)
square_vertex3 = Point(1, 1)
square_vertex4 = Point(1, 0)

square_line1 = Line(square_vertex1, square_vertex2)
square_line2 = Line(square_vertex2, square_vertex3)
square_line3 = Line(square_vertex3, square_vertex4)
square_line4 = Line(square_vertex4, square_vertex1)

square = Square([square_vertex1, square_vertex2, square_vertex3, square_vertex4],[square_line1, square_line2, square_line3, square_line4], [90, 90, 90, 90])

print("Información del triángulo equilátero:")
print("Perímetro:", equilateral_triangle.compute_perimeter())
print("Área:", equilateral_triangle.area_triangle())

print("\nInformación del triángulo isósceles:")
print("Perímetro:", isosceles_triangle.compute_perimeter())
print("Área:", isosceles_triangle.area_triangle())

print("\nInformación del triángulo escaleno:")
print("Perímetro:", scalene_triangle.compute_perimeter())
print("Área:", scalene_triangle.area_triangle())
print("Inner angles: ", scalene_triangle.compute_inner_angles())

print("\nInformación del cuadrado:")
print("Perímetro:", square.compute_perimeter())
print("Área:", square.compute_area())

```

## RESTAURANT 

``` python
class MenuItem:
    def __init__(self, name, price, size):
        self._name = name
        self._price = price
        self._size = size

    def get_name(self):
        return self._name

    def set_name(self, value):
        self._name = value

    def get_price(self):
        return self._price

    def set_price(self, value):
        self._price = value

    def get_size(self):
        return self._size

    def set_size(self, value):
        self._size = value

    def calculate_total_price(self, quantity):
        total_price = self._price * quantity
        self._price = total_price
        return total_price


class Beverage(MenuItem):
    def __init__(self, name, price, size):
        super().__init__(name, price, size)


class Appetizer(MenuItem):
    def __init__(self, name, price, size):
        super().__init__(name, price, size)


class MainCourse(MenuItem):
    def __init__(self, name, price, size, meat=True):
        super().__init__(name, price, size)
        self._meat = meat

    def get_meat(self):
        return self._meat

    def set_meat(self, value):
        self._meat = value

    def calculate_total_price(self, quantity):
        total_price = self._price * quantity
        if any(isinstance(item, Beverage) for item in order.items):
            total_price *= 0.9  
        return total_price


class Order:
    def __init__(self):
        self.items = []

    def add_item(self, item):
        self.items.append(item)

    def calculate_total_bill(self):
        total_bill = 0
        for item in self.items:
            total_bill += item.get_price()
        return total_bill

    def discount(self, amount):
        prix = self.calculate_total_bill()
        prix_discount = prix * (amount / 100)
        prix_final = prix - prix_discount
        return prix_final

    def get_items_with_prices(self):
        items_with_prices = "\n".join([f"{item.get_name()}: ${item.get_price()}" for item in self.items])
        return items_with_prices


class PayMethod:
    def __init__(self):
        pass

    def pay(self, amount):
        return amount, "Successfully paid"


class Card(PayMethod):
    def __init__(self, number, cvv, expiration_date):
        super().__init__()
        self.number = number
        self.cvv = cvv
        self.expiration_date = expiration_date


class Cash(PayMethod):
    def __init__(self):
        super().__init__()


order = Order()

jugo = Beverage(name="Jugo", price=4000, size="22 oz")
total_price_jugo = jugo.calculate_total_price(10)
order.add_item(jugo)

mazorca = Appetizer("Mazorca", 3500, "grande")
mazorca.calculate_total_price(3)
order.add_item(mazorca)

mignon = MainCourse("Fillet mignon", 34600, "grande", meat=True)
mignon.calculate_total_price(2)

item1 = Beverage("Soda", 2000, "12 oz")
item2 = Appetizer("Nachos", 5000, "Regular")
item3 = MainCourse("Spaghetti", 12000, "Large", meat=False)
item4 = Beverage("Iced Tea", 1500, "16 oz")
item5 = Appetizer("Chicken Wings", 8000, "Large")
item6 = MainCourse("Grilled Salmon", 18000, "Medium", meat=True)
item7 = Beverage("Lemonade", 2500, "20 oz")

order.add_item(mignon)
order.add_item(item1)
order.add_item(item2)
order.add_item(item3)
order.add_item(item4)
order.add_item(item5)
order.add_item(item6)
order.add_item(item7)

items_with_prices = order.get_items_with_prices()
print("Items with each price:\n", items_with_prices)

total_bill = order.calculate_total_bill()
print("Total bill amount:", total_bill)

discount = order.discount(90)
print("Discounted bill amount:", discount)

payment1 = Card(123456789, 123, "12/23")

payment1.pay(discount)

```
