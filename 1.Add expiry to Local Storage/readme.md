# Add Expiry for localStorage

## Overview

The **"Add Expiry for localStorage"** project is a simple utility for managing the expiration of items stored in `localStorage`. This utility allows you to set an expiration time for data stored in `localStorage`, ensuring that stale data is automatically removed after a specified period. This is useful for applications where data should not persist indefinitely and helps maintain optimal storage management.

## Features

- Set expiration time for items in `localStorage`
- Automatically remove expired items
- Simple and easy-to-use API
- Lightweight and dependency-free


```jS
const myLocalStorage = () => {
  const myStore = window.localStorage;

  return {
    setItem: (key, value, expiryTime) => {
      const currDate = new Date();
      const data = {
        val: value,
        expiry: currDate.getTime() + expiryTime,
      };
      myStore.setItem(key, JSON.stringify(data));
      setTimeout(() => {
        myStore.removeItem(key);
      }, expiryTime);
    },
    getItem: (key) => {
      const currTime = new Date().getTime();
      if (myStore.getItem(key) !== null) {
        const data = JSON.parse(myStore.getItem(key));
        if (currTime < data.expiry) {
          return data.val;
        }
      }

      return null;
    },
  };
};
const store = myLocalStorage();
store.setItem("foo", "bar", 5000);
setTimeout(() => {
  console.log("accessing before expiry Time", store.getItem("foo"));
}, 4000);
setTimeout(() => {
  console.log("accessing after expiry Time", store.getItem("foo"));
}, 5200);

```