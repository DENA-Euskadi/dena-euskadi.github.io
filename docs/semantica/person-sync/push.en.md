# :material-upload: PERSON-SYNC — PUSH

With the **Push** mechanism, DENA **proactively** notifies your administration whenever a person registers, changes their data, or deletes their account. It is the recommended mechanism when you need to react to changes **in real time**.

---

## How does it work?

In Push, **DENA-CORE is the client** and **your administration is the server**: DENA makes an HTTP `POST` to an endpoint you expose, carrying the change details.

``` mermaid
sequenceDiagram
    participant DENA as CORE DENA (client)
    participant Admin as Your administration (server)

    Note over DENA: Person registered / changed / deleted
    DENA->>Admin: POST <your-configured-url> (change details + event)
    Admin->>Admin: Process according to event (create / update / delete)
    Admin-->>DENA: 200 OK
```

!!! important "DENA does not impose a route"

    There is no predefined path for the push endpoint. Your administration exposes it at **the URL it has configured in DENA** for its connector. DENA will `POST` to that URL. The only requirement is that it accepts `POST` with `application/json` and returns the correct HTTP code.

---

## What must the administration implement?

!!! info "An endpoint that receives the change and acts on the event"

    1. **Expose a `POST` REST endpoint** that receives the notification JSON body.
    2. **Look at the `syncEvent` field** to know what happened and act accordingly:

    | Event | Action |
    |-------|--------|
    | `CREATED` | Create the person in your local copy |
    | `UPDATED` | Update the person's data |
    | `ID_CHANGED` | Update the NIF/NIE (locating the person by `oid`) |
    | `DELETED` | Delete the person **and their associated data** |

    3. **Respond with the HTTP code** reflecting the actual result (`200` if all went well).

---

## Why keep this local copy?

Your administration should only send change notifications ([Metadata-Sync / SRMD](../metadata-sync/index.md)) for people who **actually have a DENA account**. Push keeps your local copy up to date so that you:

- Don't send SRMD for people not in DENA.
- Stop sending SRMD for people who have deleted their account (`DELETED` event).
- Immediately have the basic data (name, contact) of newly registered people.

---

## Endpoint contract

The full specification —request body, per-event processing, response, HTTP codes and implementation checklist— is at:

[:octicons-arrow-right-24: Endpoint Person Push to Admin](./endpoints/push/endpoint-person-push-to-admin.md)

---

!!! tip "When to use Push"

    - When you need to react to person changes **in real time**.
    - When you don't want to depend on periodic files ([Pull](./pull.md)).
    - When your system needs the data immediately to notify changes via Metadata-Sync.

    !!! note "Recommendation: Push + Pull"
        We recommend implementing **both** mechanisms: Push for real time and [off-line Pull](./pull.md) as a fallback to recover any missed notifications.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
