---

# Axios in React.js: A Comprehensive Guide

Welcome to the ultimate guide to mastering Axios in React.js! This cheatsheet covers everything from basic requests to advanced error handling, retry mechanisms, and custom hooks.

## Table of Contents

1. [Introduction to Axios](#introduction-to-axios)
2. [GET Requests with Axios](#get-requests-with-axios)
3. [GET Requests with ID](#get-requests-with-id)
4. [POST Requests with Axios](#post-requests-with-axios)
5. [PUT Requests with Axios](#put-requests-with-axios)
6. [DELETE Requests with Axios](#delete-requests-with-axios)
7. [POST with JSON Body & URL Encoding](#post-with-json-body--url-encoding)
8. [Advanced Error Handling](#advanced-error-handling)
9. [Retry Mechanism](#retry-mechanism)
10. [Custom Hook for Axios Requests](#custom-hook-for-axios-requests)
11. [Handling Query Parameters](#handling-query-parameters)
12. [Using Axios Interceptors](#using-axios-interceptors)
13. [Transforming Responses with Axios](#transforming-responses-with-axios)
14. [Caching Responses with Axios](#caching-responses-with-axios)
15. [File Upload with Axios](#file-upload-with-axios)
16. [Handling Timeouts in Axios](#handling-timeouts-in-axios)
17. [Cancelling Requests with Axios](#cancelling-requests-with-axios)
18. [Monitoring Upload Progress](#monitoring-upload-progress)
19. [Handling Pagination with Axios](#handling-pagination-with-axios)
20. [Conclusion and Further Reading](#conclusion-and-further-reading)
21. [Connect with Me for More](#connect-with-me-for-more)

## Introduction to Axios

### What is Axios?

Axios is a popular JavaScript library used to make HTTP requests. It provides a simple and easy-to-use API for interacting with APIs.

### Why use Axios over Fetch API?

Axios offers features such as automatic transformation of JSON data, request and response interceptors, automatic retries, and more.

### Basic Axios Setup

```jsx
import axios from "axios";

axios
  .get("https://api.example.com/data")
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error("Error fetching data:", error);
  });
```

## GET Requests with Axios

### Simple GET Request

```jsx
import axios from "axios";

axios
  .get("https://api.example.com/items")
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error("Error fetching data:", error);
  });
```

## GET Requests with ID

### Fetching Data by ID

```jsx
import axios from "axios";

const id = 1;
axios
  .get(`https://api.example.com/items/${id}`)
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error("Error fetching item:", error);
  });
```

## POST Requests with Axios

### Simple POST Request

```jsx
import axios from "axios";

axios
  .post("https://api.example.com/items", {
    name: "New Item",
    price: 100,
  })
  .then((response) => {
    console.log("Item created:", response.data);
  })
  .catch((error) => {
    console.error("Error creating item:", error);
  });
```

## PUT Requests with Axios

### Updating Existing Data

```jsx
import axios from "axios";

const id = 1;
axios
  .put(`https://api.example.com/items/${id}`, {
    name: "Updated Item",
    price: 120,
  })
  .then((response) => {
    console.log("Item updated:", response.data);
  })
  .catch((error) => {
    console.error("Error updating item:", error);
  });
```

## DELETE Requests with Axios

### Deleting Data

```jsx
import axios from "axios";

const id = 1;
axios
  .delete(`https://api.example.com/items/${id}`)
  .then((response) => {
    console.log("Item deleted:", response.data);
  })
  .catch((error) => {
    console.error("Error deleting item:", error);
  });
```

## POST with JSON Body & URL Encoding

### Sending JSON Data

```jsx
import axios from "axios";

axios
  .post(
    "https://api.example.com/auth",
    {
      username: "user",
      password: "pass",
    },
    {
      headers: {
        "Content-Type": "application/json",
      },
    }
  )
  .then((response) => {
    const token = response.data.token;
    return axios.get("https://api.example.com/data", {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    });
  })
  .then((response) => {
    console.log("Data fetched with token:", response.data);
  })
  .catch((error) => {
    console.error("Error during request:", error);
  });
```

## Advanced Error Handling

### Handling Specific Error Cases

```jsx
import axios from "axios";

axios
  .get("https://api.example.com/data")
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    if (error.response) {
      console.error("Error response:", error.response.data);
    } else if (error.request) {
      console.error("No response received:", error.request);
    } else {
      console.error("Error setting up request:", error.message);
    }
  });
```

## Retry Mechanism

### Using Axios Interceptors for Retries

```jsx
import axios from "axios";

const axiosInstance = axios.create();

axiosInstance.interceptors.response.use(
  (response) => response,
  (error) => {
    const {
      config,
      response: { status },
    } = error;
    const originalRequest = config;

    if (status === 500) {
      return axiosInstance(originalRequest);
    }

    return Promise.reject(error);
  }
);

axiosInstance
  .get("https://api.example.com/data")
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error("Error with retry mechanism:", error);
  });
```

## Custom Hook for Axios Requests

### Encapsulating Axios Logic in a Custom Hook

```jsx
import { useState, useEffect } from "react";
import axios from "axios";

const useAxios = (url) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    axios
      .get(url)
      .then((response) => {
        setData(response.data);
        setLoading(false);
      })
      .catch((error) => {
        setError(error);
        setLoading(false);
      });
  }, [url]);

  return { data, loading, error };
};

export default useAxios;
```

## Handling Query Parameters

### Sending Requests with Query Parameters

```jsx
import axios from "axios";

axios
  .get("https://api.example.com/items", {
    params: {
      category: "electronics",
      sortBy: "price",
    },
  })
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error("Error fetching items:", error);
  });
```

## Using Axios Interceptors

### Modifying Requests with Interceptors

```jsx
import axios from "axios";

const axiosInstance = axios.create();

axiosInstance.interceptors.request.use(
  (config) => {
    config.headers["Authorization"] = "Bearer your_access_token";
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

axiosInstance
  .get("https://api.example.com/data")
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error("Error fetching data:", error);
  });
```

## Transforming Responses with Axios

### Transforming Response Data

```jsx
import axios from "axios";

axios
  .get("https://api.example.com/data", {
    transformResponse: [
      function (data) {
        return JSON.parse(data).map((item) => ({
          ...item,
          transformed: true,
        }));
      },
    ],
  })
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error("Error transforming data:", error);
  });
```

## Caching Responses with Axios

### Implementing a Simple Caching Mechanism

```jsx
import axios from "axios";

const cache = {};

const fetchData = async (url) => {
  if (cache[url]) {
    return cache[url];
  }

  try {
    const response = await axios.get(url);
    cache[url] = response.data;
    return response.data;
  } catch (error) {
    console.error("Error fetching data:", error);
    throw error;
  }
};

fetchData("https://api.example.com/data").then((data) => {
  console.log(data);
});
```

## File Upload with Axios

### Uploading Files

```jsx
import axios from "axios";

const formData = new FormData();
formData.append("file", fileInput.files[0]);

axios
  .post("https://api.example.com/upload", formData, {
    headers: {
      "Content-Type": "multipart/form-data",
    },
  })
  .then((response) => {
    console.log("File uploaded successfully:", response.data);
  })
  .catch((error) => {
    console.error("Error uploading file:", error);
  });
```

## Handling Timeouts in Axios

### Setting and Handling Request Timeouts

```jsx
axios
  .get("https://api.example.com/data", {
    timeout: 5000, // 5 seconds timeout
  })
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    if (error.code === "ECONNABORTED") {
      console.error("Request timed out:", error.message);
    } else {
      console.error("Error fetching data:", error);
    }
  });
```

## Cancelling Requests with Axios

### Using Cancel Tokens

```jsx
import axios from "axios";

const source = axios.CancelToken.source();

axios
  .get("https://api.example.com/data", {
    cancelToken: source.token,
  })
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    if (axios.isCancel(error)) {
      console.log("Request canceled", error.message);
    } else {
      console.error("Error fetching data:", error);
    }
  });

// Cancel the request
source.cancel("Operation canceled by the user.");
```

## Monitoring Upload Progress

### Tracking Upload Progress

```jsx
import axios from "axios";

const formData = new FormData();
formData.append("file", fileInput.files[0]);

axios
  .post("https://api.example.com/upload", formData, {
    headers: {
      "Content-Type": "multipart/form-data",
    },
    onUploadProgress: (progressEvent) => {
      const percentCompleted = Math.round(
        (progressEvent.loaded * 100) / progressEvent.total
      );
      console.log(`Upload progress: ${percentCompleted}%`);
    },
  })
  .then((response) => {
    console.log("File uploaded successfully:", response.data);
  })
  .catch((error) => {
    console.error("Error uploading file:", error);
  });
```

## Handling Pagination with Axios

### Fetching Paginated Data

```jsx
import axios from "axios";

const fetchPage = async (page) => {
  try {
    const response = await axios.get("https://api.example.com/items", {
      params: {
        page: page,
        limit: 10,
      },
    });
    console.log(`Page ${page} data:`, response.data);
  } catch (error) {
    console.error("Error fetching paginated data:", error);
  }
};

fetchPage(1);
```

## Conclusion and Further Reading

### Wrapping Up

In this guide, we've covered a wide range of topics related to using Axios in React.js. From basic requests to advanced features like interceptors, retries, and custom hooks, you should now have a solid understanding of how to effectively use Axios in your projects.

### Further Reading

- [Axios Documentation](https://axios-http.com/docs/intro)
- [React.js Documentation](https://reactjs.org/docs/getting-started.html)
- [JavaScript Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)

## Connect with Me for More

### Stay in Touch

If you found this guide helpful and want to stay updated with more tutorials and tips, feel free to connect with me on:

- [GitHub](https://github.com/Sabariramamoorthy)
- [LinkedIn](https://www.linkedin.com/in/sabarimani-r/)

Happy coding!
