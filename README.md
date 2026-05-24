# Photobooth : Raspberry Pi Powered Photo System

A fully automated photobooth system built on Raspberry Pi : from QR code payment to filtered photo printing.

---

## How it works

1. **QR Code display** : The Raspberry Pi calls the Mollie API and waits for payment confirmation
2. **Payment** : The Raspberry Pi receives payment validation via webhook / HTTPS polling every 2 seconds
3. **Filter selection** : The user picks a filter on the touchscreen
4. **Photo capture** : The Raspberry Pi triggers the Canon EOS M100 via USB (gPhoto2), with a countdown displayed on screen
5. **Preview** : The filtered photo is previewed on the touchscreen before printing
6. **Printing** : The Raspberry Pi sends the file to the Canon Selphy CP1500 via USB

---

## Architecture

```
Touchscreen (HDMI)
    |
Raspberry Pi ── USB ── Canon EOS M100 (camera)
    |
    └── USB ── Canon Selphy CP1500 (printer)

Mollie API (QR Code payment) ──> Raspberry Pi (webhook / polling)
```

The Raspberry Pi is the central brain : it handles payment, triggers the camera, applies filters, manages the UI and sends the final file to the printer.

---

## Hardware

| Component | Model | Cost |
|-----------|-------|------|
| Single-board computer | Raspberry Pi 4B (4GB) | ~120€ |
| Touchscreen | Freenove 7" IPS (800x480, DSI) | ~55€ |
| Camera | Canon EOS M100 | already on-site |
| Printer | Canon Selphy CP1500 | ~142€ |
| Paper + ink pack | Canon RP-108 (4x6", 108 sheets) | ~34€ |
| Misc (cables, SD cards, power) | USB, HDMI, microSD x2 | ~50€ |
| **Total** | | **~401€** |

**Cost per print: ~0.30€**

Paper and ink refills available at Amazon and MediaMarkt.

---

## Software Stack

| Component | Details |
|-----------|---------|
| **OS** | Raspberry Pi OS |
| **Web interface** | Flask (Python) served locally |
| **Payment** | Mollie API POST request, QR code generation, webhook + HTTPS polling every 2s |
| **Camera control** | gPhoto2 USB trigger, live preview, file retrieval |
| **Printing** | CUPS + Selphy driver standard Linux print manager |
| **Image filters** | Pillow custom filters + room-specific overlays |
| **Inter-component communication** | HTTP polling every Xms with JSON |
| **Boot management** | systemd / crontab auto-start on boot |

---

## Flask UI Flow

1. **Home screen** : button + price display
2. **Payment screen** : QR code + amount
3. **Filter selection** : filter menu + live preview
4. **Countdown** : 5 second timer before capture
5. **Preview** : filtered photo preview before printing
6. **Confirmation screen** : print confirmation

---

## Error Handling

- Payment timeout
- Camera not detected
- Print failure
- Errors caught during script execution and identified during testing phases

---

## Development

**Estimated build time: 3 weeks**

**Phases:**
- Canon EOS M100 + gPhoto2 compatibility testing
- Mollie integration (test API first, then live API)
- Flask UI development
- Filter implementation with Pillow
- CUPS + Selphy printer configuration
- Extended testing for each step of the flow
- Error handling implementation
- Auto-start configuration (systemd)

---

## Notes

> ⚠️ The Canon EOS M100 may have compatibility issues with gPhoto2 for USB trigger — needs testing before finalizing hardware choice.
