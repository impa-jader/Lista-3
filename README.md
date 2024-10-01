# Jader Duarte - Lista 3

# Coisas da lista 1
class Field:
    pass

class VectorSpace:
    """VectorSpace:
    Abstract Class of vector space used to model basic linear structures
    """
    
    def __init__(self, dim: int, field: 'Field'):
        """
        Initialize a VectorSpace instance.

        Args:
            dim (int): Dimension of the vector space.
            field (Field): The field over which the vector space is defined.
        """
        self.dim = dim
        self._field = field
        
    def getField(self):
        """
        Get the field associated with this vector space.

        Returns:
            Field: The field associated with the vector space.
        """
        return self._field
    
    def getVectorSpace(self):
        """
        Get a string representation of the vector space.

        Returns:
            str: A string representing the vector space.
        """
        return f'dim = {self.dim!r}, field = {self._field!r}'
        # return self.__repr__()

    def __repr__(self):
        """
        Get a string representation of the VectorSpace instance.

        Returns:
            str: A string representing the VectorSpace instance.
        """
        # return f'dim = {self.dim!r}, field = {self._field!r}'
        return self.getVectorSpace()
    
    def __mul__(self, f):
        """
        Multiplication operation on the vector space (not implemented).

        Args:
            f: The factor for multiplication.

        Raises:
            NotImplementedError: This method is meant to be overridden by subclasses.
        """
        raise NotImplementedError
    
    def __rmul__(self, f):
        """
        Right multiplication operation on the vector space (not implemented).

        Args:
            f: The factor for multiplication.

        Returns:
            The result of multiplication.

        Note:
            This method is defined in terms of __mul__.
        """
        return self.__mul__(f)
    
    def __add__(self, v):
        """
        Addition operation on the vector space (not implemented).

        Args:
            v: The vector to be added.

        Raises:
            NotImplementedError: This method is meant to be overridden by subclasses.
        """
        raise NotImplementedError

class RealVector(VectorSpace):
    _field = float
    def __init__(self, dim, coord):
        super().__init__(dim, self._field)
        self.coord = coord
    

    @staticmethod
    def _builder(coord):
        raise NotImplementedError


    def __add__(self, other_vector):
        n_vector = []
        for c1, c2 in zip(self.coord, other_vector.coord):
            n_vector.append(c1+c2)
        return self._builder(n_vector)


    def __mul__(self, alpha):
        n_vector = []
        for c in self.coord:
            n_vector.append(alpha*c)
        return self._builder(n_vector)
    
    
    def iner_prod(self, other_vector):
        res = 0
        for c1, c2 in zip(self.coord, other_vector.coord):
            res += c1*c2
        return res


    def __str__(self):
        ls = ['[']
        for c in self.coord[:-1]:
            ls += [f'{c:2.2f}, ']
        ls += f'{self.coord[-1]:2.2f}]'
        s =  ''.join(ls)
        return s


class Vector2D(RealVector):
    _dim = 2
    def __init__(self, coord):
        if len(coord) != 2:
            raise ValueError
        super().__init__(self._dim, coord)


    @staticmethod
    def _builder(coord):
        return Vector2D(coord)
    

    def CW(self):
        return Vector2D([-self.coord[1], self.coord[0]])
    

    def CCW(self):
        return Vector2D([self.coord[1], -self.coord[0]])
    #2
    def __add__(self, other__vector):
        return super().__add__(other__vector)
    def __mul__(self, alpha):
        return super().__mul__(alpha)
    def __rmul__(self, f):
        return super().__rmul__(f)
    def __sub__(self,other__vector):
        return super().__add__(other__vector*(-1))
    def __abs__(self):
        r=0
        for i in range(self._dim):
            r+=self.coord[i]**2
        return r**(1/2)
#Questão 3 da lista 1. Questão 2 lista atual
class Vector3D(RealVector):
    _dim = 3
    def __init__(self, coord):
        if len(coord) != 3:
            raise ValueError
        super().__init__(self._dim, coord)


    @staticmethod
    def _builder(coord):
        return Vector3D(coord)
    

    """def CW(self):
        return Vector3D([-self.coord[1], self.coord[0]])
    

    def CCW(self):
        return Vector3D([self.coord[1], -self.coord[0]])"""
    
    def __add__(self, other__vector):
        return super().__add__(other__vector)
    def __mul__(self, alpha):
        return super().__mul__(alpha)
    def __rmul__(self, f):
        return super().__rmul__(f)
    def __sub__(self,other__vector):
        return super().__add__(other__vector*(-1))
    def __abs__(self):
        r=0
        for i in range(self._dim):
            r+=self.coord[i]**2
        return r**(1/2)
    # Questão 3
    # definirei eps como 0.001 porque sim. É um numero aleatorio
    def __eq__(self, other_verctor):
        eps= 0.001
        for i in range(3):
            if abs(self.coord[i] - other_verctor.coord[i])> eps:
                return False
        return True
    def __lt__(self,other_vector):
        eps= 0.001
        if abs(self)-abs(other_vector)< -eps:
            return True
        else:
            return False
    def __gt__(self,other_vector):
        eps=0.001
        if abs(self)-abs(other_vector)>eps:
            return True
        else:
            return False
    def __le__(self,other_vector):
        if self==other_vector:
            return True
        elif self<other_vector:
            return True
        else:
            return False
    def __ge__(self,other_vector):
        if not self<other_vector:
            return True
        else:
            return False

if __name__=="__main__":
    print(Vector3D([1,3,4])==Vector3D([1,3,4]))
    print(Vector3D([1,3,4])<Vector3D([8,3,4]))
    print(Vector3D([1,3,4])>Vector3D([8,3,4]))
    print(Vector3D([1,3,4])<=Vector3D([1,3,4]))
    print(Vector3D([1.0001,3.000004,4.0003])==Vector3D([1,3,4]))
    
'''questão 3'''
def find_epsilon():
    x=1
    while x+1!=1:
        x=x/10
    k=1
    while 1+k*x==1:
        k+=0.01
    return x*k
print(find_epsilon())

import numpy as np
epsilon = np.finfo(float).eps
print(epsilon)
