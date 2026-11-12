# LoRa soldier tracking

## A bit of background

Soldiers in remote areas often operate where there's no mobile network and no Wi-Fi, so ordinary phones and trackers don't help. Military radios work, but they're expensive and power-hungry. What's needed is something cheap and low-power that can still send small amounts of data, such as a location or a status, over a long distance.

LoRa (short for Long Range) is a radio technology built for exactly this. It sends data slowly and in small packets, but it can reach kilometres in open areas while running on small batteries. That's why it's used in things like smart farming sensors and city-wide meters. A tracking message is tiny, so LoRa's low data rate isn't a problem here.

## About this project

Two Arduino Unos talking over LoRa. One acts as the soldier's unit and sends its location, the other is the base station that receives it.

I first built this in college and published it as a paper: *Secure LoRa Wireless Communication for Soldier Tracking with the Help of Interfacing Two Arduino Uno Devices in Military Operations*, [journal/conference, year, link]. The original code isn't available anymore, so in 2026 I rebuilt it from scratch, and this time I'm going further: real GPS data, encrypted packets and proper range testing.

## Hardware

- 2 × Arduino Uno
- 2 × [LoRa module, e.g. Ra-02 / SX1278], powered from 3.3 V through a logic level shifter
- 1 × NEO-6M GPS module (on the transmitter)

Wiring:

| LoRa | Uno |
|---|---|
| [NSS] | [D10] |
| [MOSI] | [D11] |
| [MISO] | [D12] |
| [SCK] | [D13] |
| [RST] | [D9] |
| [DIO0] | [D2] |

| GPS | Uno |
|---|---|
| [TX] | [D4] |
| [RX] | [D3] |

LoRa settings: [frequency] MHz, SF[ ], [ ] kHz bandwidth.

![setup](docs/photos/setup.jpg)

## Packet

| Field | Size | |
|---|---|---|
| Node ID | 1 byte | which unit sent it |
| Counter | 4 bytes | goes up by one every packet, used to reject replayed packets |
| Latitude | 4 bytes | |
| Longitude | 4 bytes | |
| [Status] | [ ] | |

## Running it

Libraries: LoRa (Sandeep Mistry), TinyGPSPlus, Crypto (Rhys Weatherley).
Flash `firmware/transmitter` to one board and `firmware/receiver` to the other, then open the serial monitor on the receiver at [baud] baud.

## Progress

- [ ] v1: basic link, receiver prints each packet with RSSI and SNR
- [ ] v2: live GPS location
- [ ] v3: AES-128 encrypted packets + replay protection using the counter
- [ ] v4: range test, packets received and signal strength at different distances, SF7 vs SF12

<!-- ## Range test results

[Fill in after v4: where you tested, distances, packets received, RSSI.]

## Things I ran into

[In your words.] --!>
