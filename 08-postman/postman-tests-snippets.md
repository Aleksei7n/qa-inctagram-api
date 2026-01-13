# Postman Tests — Snippets

Версия: 1.1  

---

## 1) Status code + response time
```js
pm.test("Status code is 200", () => pm.response.to.have.status(200));
pm.test("Response time < 1500ms", () => {
  pm.expect(pm.response.responseTime).to.be.below(1500);
});

---

## JSON basic contract

pm.test("Response is JSON", () => pm.response.to.be.json);

const json = pm.response.json();
pm.expect(json).to.be.an("object");

---

## Extract accessToken from login and set env var

pm.test("Login returns accessToken", () => {
  pm.response.to.have.status(200);

  const json = pm.response.json();
  pm.expect(json).to.have.property("accessToken");

  pm.environment.set("bearerToken", json.accessToken);
});

---

## Unauthorized (negative) — without token

pm.test("Unauthorized without token", () => {
  pm.response.to.have.status(401);
});

---

## 204 No Content

pm.test("No Content", () => pm.response.to.have.status(204));
pm.test("Empty body", () => {
  pm.expect(pm.response.text()).to.be.oneOf(["", "null"]);
});

---

## Basic field/type checks

const json = pm.response.json();

// пример: массив items
pm.expect(json).to.have.property("items");
pm.expect(json.items).to.be.an("array");

// пример: первый элемент массива
if (json.items.length > 0) {
  pm.expect(json.items[0]).to.have.property("id");
  pm.expect(json.items[0].id).to.be.a("string");
}
