### ¿Qué son los Query Parameter Families?

Los Query Parameter Families son una forma de organizar y estructurar los parámetros de consulta en una URL dentro de una API web. Esto se hace para agrupar parámetros relacionados y facilitar la comprensión y el uso de la API.

### ¿Dónde se utilizan?

En el contexto de una API RESTful, es común utilizar parámetros de consulta para filtrar, ordenar, paginar o limitar los resultados de las solicitudes HTTP. Al organizar estos parámetros en familias, se puede tener un conjunto claro de categorías de parámetros que tienen un propósito específico.

### Ejemplo:

```bash
/api/restaurants?location[city]=New_York&location[state]=NY&cuisine[type]=Italian&cuisine[vegan]=true
```

Los parámetros de consulta están organizados en dos familias:

- `location` es la familia de parámetros utilizada para especificar la ubicación de búsqueda. Dentro de esta familia:

  - `city` se utiliza para indicar la ciudad, que es "New York".
  - `state` se utiliza para especificar el estado, que es "NY".

- `cuisine` es otra familia de parámetros utilizada para definir preferencias de comida. Dentro de esta familia:
  - `type` se utiliza para especificar el tipo de cocina deseado, que es "Italian" (italiana).
  - `vegan` es un parámetro booleano que se establece en `true` para buscar restaurantes que ofrezcan opciones veganas.

### Obtener los valores:

```typescript
import { NextApiRequest, NextApiResponse } from 'next';

interface RestaurantQuery {
  "location[city]": string;
  "location[state]": string;
  "cuisine[type]": string;
  "cuisine[vegan]": string;
}

// Definimos una función de controlador de tipo flecha exportada por defecto
const getRestaurantsController = async (req: NextApiRequest, res: NextApiResponse) => {
  try {
    // Parseamos los parámetros de consulta de la solicitud
    const {
      "location[city]": city,
      "location[state]": state,
      "cuisine[type]": cuisineType,
      "cuisine[vegan]": isVegan
    } = req.query as unknown as RestaurantQuery;

    // Aquí puedes realizar la lógica para obtener datos de restaurantes
    // por ejemplo, consultando una base de datos o llamando a una API externa.
    // En este ejemplo, simplemente devolvemos los parámetros de consulta como respuesta.

    const responseData = {
      location: {
        city,
        state,
      },
      cuisine: {
        type: cuisineType,
        vegan: isVegan,
      },
    };

    res.status(200).json(responseData);
  } catch (error) {
    console.error('Error en el controlador:', error);
    res.status(500).json({ error: 'Se produjo un error en el servidor' });
  }
};

export default getRestaurantsController;
```
