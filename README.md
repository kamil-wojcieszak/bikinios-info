# bikinios

**PL** · [EN](#english)

`bikinios` to wewnętrzna aplikacja sklepu ze strojami kąpielowymi Bikinios. Łączy nasz system ERP (wFirma) z kontem sprzedawcy na Allegro. Nie jest dostępna publicznie i działa tylko na jednym koncie sprzedawcy.

## User-Agent

Każde zapytanie do REST API Allegro wysyłamy z nagłówkiem:

```
User-Agent: bikinios/<wersja> (+https://github.com/kamil-wojcieszak/bikinios-info)
```

`<wersja>` to wersja aktualnie wdrożona na produkcję.

## Co robi aplikacja

- **Oferty:** wystawia nowe produkty z naszego katalogu i aktualizuje stany magazynowe, gdy zmieniają się w wFirmie.
- **Zamówienia:** pobiera zdarzenia zamówień i formularze zakupowe, a potem aktualizuje status realizacji.
- **Wysyłka:** tworzy przesyłki w „Wysyłam z Allegro”, pobiera etykiety i zamawia odbiór paczek.
- **Zwroty:** pobiera zwroty klientów i zleca zwrot płatności.

Używane zasoby API: `/sale/product-offers`, `/sale/offers`, `/sale/offer-events`, `/sale/products`, `/sale/categories/{id}/parameters`, `upload.allegro.pl/sale/images`, `/order/events`, `/order/checkout-forms`, `/order/customer-returns`, `/payments/refunds`, `/shipment-management/*`.

## Natężenie ruchu

- Zdarzenia zamówień: co 5 minut.
- Planowanie nowych ofert: raz w tygodniu (poniedziałek rano).
- Synchronizacja sprzedaży: raz na dobę w nocy.
- Pozostałe zapytania wywołuje obsługa magazynu z panelu lub zmiana stanu w wFirmie.

Po odpowiedzi `429 Too Many Requests` ponawiamy zapytanie z rosnącym odstępem (do 4 prób).

## Kontakt

<!-- TODO: uzupełnij adres kontaktowy -->
E-mail: `kontakt@example.com`

---

<a id="english"></a>

## English

`bikinios` is the internal back-office application of Bikinios, a swimwear shop. It connects our ERP (wFirma) to our Allegro seller account. It is not publicly available and runs for a single seller account only.

**User-Agent:** `bikinios/<version> (+https://github.com/kamil-wojcieszak/bikinios-info)`, where `<version>` is the version currently deployed to production.

**What it does:**
- lists our products as offers and keeps their stock in sync with wFirma;
- reads order events and checkout forms and updates fulfillment status;
- creates "Wysyłam z Allegro" shipments, fetches labels and books pickups;
- reads customer returns and issues refunds.

**Traffic:**
- Order events: polled every 5 minutes.
- New-offer planning: weekly.
- Sales sync: nightly.
- Everything else is triggered by warehouse staff or by stock changes in wFirma.

We retry `429` responses with backoff, up to 4 attempts.

**Contact:** see above.
