Peacefair PZEM-00X DC Energy Monitor
====================================

.. seo::
    :description: Instructions for setting up DC PZEM power monitors.
    :image: pzem-dc.jpg

.. note::

    This page is incomplete and could use some work. If you want to contribute, please read the
    :doc:`contributing guide </guides/contributing>`. This page is missing:

      - Images/screenshots/example configs of this device being used in action.

The ``pzemdc`` sensor platform allows you to use various DC Peacefair PZEM energy monitors
with ESPHome. The supported models are: PZEM-003, PZEM-014, PZEM-016 and PZEM-017.

The communication with this component is via a :ref:`UART <uart>`.
You must therefore have a ``uart:`` entry in your configuration with both the TX and RX pins set
to some pins on your board and the baud rate set to 9600.

To use these devices with ESPHome, you will need an RS485 to UART TTL converter. 
If your converter does not include a built-in 120-ohm resistor, you may experience unreliable readings or "unknown" values in Home Assistant. 
In such cases, connect a 120-ohm resistor between the A and B terminals of the RS485 bus.

These sensors require an independent power supply to operate if the Test input Voltage is lower then 7v. The USB input powers the internal measurement circuit, while the RS485 communication circuit requires a separate power supply via the screw terminal block. ( tested on PZEM-017 )
For isolation and safety, use different power sources for these circuits. For example, a 5V 1A adapter can power the USB input externally , while the RS485 input can be powered with ESP32 5V supply with a common Ground . 
This separation helps prevent potential interference and ensures Safe operation.

The PZEM-017 sensors use an external current shunt to measure current and power. The default shunt value from the factory is 100A. 
If you need to use a different shunt (e.g., 50A, 200A, or 300A), you must configure this value using the manufacturer's provided software. 
The shunt value must match the actual shunt installed to ensure accurate readings. 

"The PZEM-003 and PZEM-014 do not require an external shunt and can measure up to 10 amps, while the PZEM-016 is an AC meter that uses a CT (current transformer)."


.. figure:: images/pzem-dc.png
    :align: center
    :width: 80.0%

    PZEM-0xx Energy Monitor.

.. code-block:: yaml

    # Example configuration entry
    uart:
      tx_pin: D1
      rx_pin: D2
      baud_rate: 9600
      stop_bits: 2

    sensor:
      - platform: pzemdc
        current:
          name: "PZEM-003 Current"
        voltage:
          name: "PZEM-003 Voltage"
        power:
          name: "PZEM-003 Power"
        energy:
          name: "PZEM-003 Energy"
        update_interval: 60s

Configuration variables:
------------------------

- **current** (*Optional*): Use the current value of the sensor in amperes. All options from
  :ref:`Sensor <config-sensor>`.
- **power** (*Optional*): Use the power value of the sensor in watts. All options from
  :ref:`Sensor <config-sensor>`.
- **voltage** (*Optional*): Use the voltage value of the sensor in volts.
  All options from :ref:`Sensor <config-sensor>`.
- **energy** (*Optional*): Use the energy value of the sensor in kWh.
  All options from :ref:`Sensor <config-sensor>`.
- **update_interval** (*Optional*, :ref:`config-time`): The interval to check the
  sensor. Defaults to ``60s``.
- **address** (*Optional*, int): The address of the sensor if multiple sensors are attached to
  the same UART bus. You will need to set the address of each device manually. Defaults to ``1``.

.. _pzemdc-reset_energy_action:

``pzemdc.reset_energy`` Action
******************************

This action resets the total energy value of the pzemdc device with the given ID when executed.

.. code-block:: yaml

    on_...:
      then:
        - pzemdc.reset_energy: pzemdc_1


See Also
--------

- :ref:`sensor-filters`
- :doc:`pzem004t`
- :doc:`pzemac`
- :apiref:`pzemdc/pzemdc.h`
- :ghedit:`Edit`
