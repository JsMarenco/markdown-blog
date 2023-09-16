## What are Query Parameter Families?

Query Parameter Families are a way to organize and structure query parameters in a URL within a web API. This is done to group related parameters and make the API more understandable and user-friendly.

## Where are they used?

In the context of a RESTful API, it is common to use query parameters to filter, sort, paginate, or limit the results of HTTP requests. By organizing these parameters into families, you can have a clear set of parameter categories that serve a specific purpose.

## Example:

```bash
/api/restaurants?location[city]=New_York&location[state]=NY&cuisine[type]=Italian&cuisine[vegan]=true
```

The query parameters are organized into two families:

- `location` is the parameter family used to specify the search location. Within this family:
  - `city` is used to indicate the city, which is "New York."
  - `state` is used to specify the state, which is "NY."

- `cuisine` is another parameter family used to define food preferences. Within this family:
  - `type` is used to specify the desired cuisine type, which is "Italian."
  - `vegan` is a boolean parameter set to `true` to search for restaurants that offer vegan options.

### Get query values:

```typescript
import { NextApiRequest, NextApiResponse } from 'next';

interface RestaurantQuery {
  "location[city]": string;
  "location[state]": string;
  "cuisine[type]": string;
  "cuisine[vegan]": string;
}

// Define a default exported arrow function controller
const getRestaurantsController = async (req: NextApiRequest, res: NextApiResponse) => {
  try {
    // Parse the query parameters from the request
    const { 
      "location[city]": city, 
      "location[state]": state, 
      "cuisine[type]": cuisineType, 
      "cuisine[vegan]": isVegan 
    } = req.query as unknown as RestaurantQuery;

    // Here, you can perform logic to retrieve restaurant data,
    // for example, querying a database or calling an external API.
    // In this example, we simply return the query parameters as a response.

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
    console.error('Error in the controller:', error);
    res.status(500).json({ error: 'An error occurred on the server' });
  }
};

export default getRestaurantsController;
```