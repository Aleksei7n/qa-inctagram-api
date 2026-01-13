# Postman Tests — Snippets (Inctagram/Somegram API)

Версия: 1.2  
Переменные окружения: `{{baseUrl}}`, `{{bearerToken}}`

---

```js

0) Helpers

// ===== Helpers =====
function expectStatusIn(codes) {
  pm.test(`Status is one of [${codes.join(", ")}]`, () => {
    pm.expect(codes).to.include(pm.response.code);
  });
}

function expectResponseTime(maxMs) {
  pm.test(`Response time < ${maxMs}ms`, () => {
    pm.expect(pm.response.responseTime).to.be.below(maxMs);
  });
}

function expectJson() {
  pm.test("Content-Type includes application/json", () => {
    const ct = (pm.response.headers.get("Content-Type") || "").toLowerCase();
    pm.expect(ct).to.include("application/json");
  });
  pm.test("Response is JSON", () => pm.response.to.be.json);
}

function safeJson() {
  try { return pm.response.json(); } catch (e) { return null; }
}

function expectNot500() {
  pm.test("Server should not return 500", () => {
    pm.expect(pm.response.code).to.not.eql(500);
  });
}

function expectEmptyBody() {
  pm.test("Empty body (204)", () => {
    pm.expect(pm.response.text()).to.be.oneOf(["", "null"]);
  });
}


⸻

1) Status code + response time

// ===== (1) Status code + response time =====
expectStatusIn([200]);
expectResponseTime(1500);


⸻

2) JSON basic contract

// ===== (2) JSON basic contract =====
expectJson();

const json = safeJson();
pm.test("Body exists and is object/array", () => {
  pm.expect(json).to.not.equal(null);
  pm.expect(["object", "array"]).to.include(Array.isArray(json) ? "array" : typeof json);
});


⸻

3) AUTH — Login: extract accessToken

// ===== (3) AUTH Login: extract accessToken =====
expectStatusIn([200]);
expectJson();

const json = safeJson();

pm.test("accessToken exists and is string", () => {
  pm.expect(json).to.be.an("object");
  pm.expect(json).to.have.property("accessToken");
  pm.expect(json.accessToken).to.be.a("string").and.not.empty;
});

pm.environment.set("bearerToken", json.accessToken);


⸻

4) Unauthorized (401) — without token

// ===== (4) Unauthorized without token =====
expectStatusIn([401]);


⸻

5) 204 No Content

// ===== (5) 204 No Content =====
expectStatusIn([204]);
expectEmptyBody();


⸻

6) Validation error schema (422)

// ===== (6) Validation error schema (422) =====
expectStatusIn([422]);
expectJson();
expectNot500();

const json = safeJson();

pm.test("422 schema: statusCode + message + errors[]", () => {
  pm.expect(json).to.be.an("object");
  pm.expect(json).to.have.property("statusCode");
  pm.expect(json).to.have.property("message");
  pm.expect(json).to.have.property("errors");
  pm.expect(json.errors).to.be.an("array");
});

if (safeJson() && Array.isArray(safeJson().errors) && safeJson().errors.length > 0) {
  pm.test("422 errors[0] has property + constraints", () => {
    const e = safeJson().errors[0];
    pm.expect(e).to.have.property("property");
    pm.expect(e.property).to.be.a("string");
    pm.expect(e).to.have.property("constraints");
    pm.expect(e.constraints).to.be.an("object");
  });
}


⸻

7) Bad request schema (400)

// ===== (7) Bad Request schema (400) =====
expectStatusIn([400]);
expectJson();
expectNot500();

const json = safeJson();

pm.test("400 schema: status + error + message", () => {
  pm.expect(json).to.be.an("object");
  pm.expect(json).to.have.property("status");
  pm.expect(json).to.have.property("error");
  pm.expect(json).to.have.property("message");
});


⸻

8) DEVICES — list contract

// ===== (8) DEVICES: list contract =====
expectStatusIn([200]);
expectJson();

const json = safeJson();

pm.test("Body is array", () => {
  pm.expect(json).to.be.an("array");
});

if (Array.isArray(json) && json.length > 0) {
  pm.test("devices[0] has required fields", () => {
    const d = json[0];
    pm.expect(d).to.have.property("deviceId");
    pm.expect(d).to.have.property("deviceName");
    pm.expect(d).to.have.property("ip");
    pm.expect(d).to.have.property("lastVisit");

    pm.expect(d.deviceId).to.be.a("string");
    pm.expect(d.deviceName).to.be.a("string");
    pm.expect(d.ip).to.be.a("string");
    pm.expect(d.lastVisit).to.be.a("string");
  });
}


⸻

9) AUTH — reCAPTCHA site key

// ===== (9) AUTH reCAPTCHA site key =====
expectStatusIn([200]);
expectJson();

const json = safeJson();

pm.test("Body has recaptchaSiteKey", () => {
  pm.expect(json).to.be.an("object");
  pm.expect(json).to.have.property("recaptchaSiteKey");
  pm.expect(json.recaptchaSiteKey).to.be.a("string").and.not.empty;
});


⸻

10) Invalid ID — expect 400/404 (not 500)

// ===== (10) Invalid ID should return 400/404 (not 500) =====
expectStatusIn([400, 404]);
expectNot500();
