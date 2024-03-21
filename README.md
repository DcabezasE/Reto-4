# Reto-4
    from math import acos
    from math import degrees
    
    class Point:
        def __init__(self, x, y):
            self.x = x
            self.y = y
    
        def compute_distance(self, point):
    
            dx = self.x - point.x
            dy = self.y - point.y
            return (dx**2 + dy**2)**0.5
    
    
    class Line:
        def __init__(self, start_point, end_point):
            self.start_point = start_point
            self.end_point = end_point
            self.length = start_point.compute_distance(end_point)
    
    
    class Shape:
        def __init__(self, vertices):
            self.vertices = vertices
            self.edges = [Line(vertices[i], vertices[(i + 1) % len(vertices)]) for i in range(len(vertices))]
            self.inner_angles = self.compute_inner_angles()
            self.regular = self.compute_regular()
        def compute_regular(self):
            regular = False
            v1=0
            v2=0
            for edge in self.edges:
                v2 = edge.start_point.compute_distance(edge.end_point)
                if v1 != 0:
                    if v1 == v2:
                        regular=True
                        v1 = v2
                    else:
                        regular= False
                else:
                    v1 = v2
            return regular
    
    
        def compute_inner_angles(self):
    
            angles = []
            for i in range(len(self.vertices)):
                prev_point = self.vertices[i - 1]
                current_point = self.vertices[i]
                next_point = self.vertices[(i + 1) % len(self.vertices)]
    
    
                v1 = Point(prev_point.x - current_point.x, prev_point.y - current_point.y)
                v2 = Point(next_point.x - current_point.x, next_point.y - current_point.y)
    
    
                dot_product = v1.x * v2.x + v1.y * v2.y
                magnitude_v1 = (v1.x**2 + v1.y**2)**0.5
                magnitude_v2 = (v2.x**2 + v2.y**2)**0.5
                angle_rad = acos(dot_product / (magnitude_v1 * magnitude_v2))
                angle_deg = degrees(angle_rad)
    
                angles.append(angle_deg)
    
            return angles
    
        def compute_area(self, shape_edges):
            if len(self.vertices) == 3:
                H=[]
                for i in self.vertices:
                    H.append(i.y)
                H.sort(reverse=True)
                Height=H[0]
                for edge in shape_edges:
                    Base = edge.start_point.compute_distance(edge.end_point)
                    break
                print(f"El area es : {(Height * Base)/2}")
            elif len(self.vertices) == 4:
                E=[]
                for edge in shape_edges:
                    E.append(edge.start_point.compute_distance(edge.end_point))
                Base=E[0]
                Height=E[1]
                print(f"El area es: {Base*Height}")
    
        def compute_perimeter(self, shape_edges):
            perimeter = 0
            for edge in shape_edges:
                perimeter += edge.start_point.compute_distance(edge.end_point)
    
            print(f"El perimetro es : {perimeter:.2f}" )
    
    
    
    class Triangle(Shape):
        def __init__(self, vertices):
            super().__init__(vertices)
    
    
    class Isosceles(Triangle):
        pass
    
    class Equilateral(Triangle):
        def __init__(self):
            if self.regular == True:
                print("Es Equilatero")
            else:
                pass
    
    class Scalene(Triangle):
        pass
    
    class TriRectangle(Triangle):
        pass
    
    
    
    class Rectangle(Shape):
        def __init__(self, vertices):
            super().__init__(vertices)
    
    class Square(Rectangle):
        pass
    
    
    if __name__ == "__main__":
    
        triangle_vertices = [Point(0, 0), Point(4, 0), Point(2, 2)]
        triangle = Triangle(triangle_vertices)
    
        print("Ángulos internos del triángulo:", triangle.inner_angles)
        print("Longitudes de los lados del triángulo:")
        for edge in triangle.edges:
            print(f"{edge.start_point.compute_distance(edge.end_point):.2f}")
    
        triangle.compute_perimeter(triangle.edges)
        triangle.compute_area(triangle.edges)
    
    
        print("Es regular?", triangle.regular)
    
    
        square_vertices = [Point(0, 0), Point(4, 0), Point(4, 4), Point(0, 4)]
        square = Square(square_vertices)
    
        print("Ángulos internos del cuadrado:", square.inner_angles)
        print("Longitudes de los lados del cuadrado:")
        for edge in square.edges:
            print(f"{edge.start_point.compute_distance(edge.end_point):.2f}")
        square.compute_perimeter(square.edges)
        square.compute_area(square.edges)
    
        print("Es regular?", square.regular)
