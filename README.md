# homeassistant-blueprints-loadbalancing

Before installing your blueprint, make sure that:

1. Current is measured (Three Phase). If single phase, please add the single phase entity on all three items.
2. Easee dynamic current limits are unlocked (See figure below). Status - should be visualised and that the actual charger current is activated.
![configure_easee](https://user-images.githubusercontent.com/35264175/202920195-e1cf29a3-9329-4115-b1e0-176ee4b2b054.png)




Currents can also be calculated by defining your own signals in configuration.yaml. Look at the example below. With this code "charge_current_limit_easee" can instead be used 


sensor:
  - platform: template
    sensors:
      remaining_current_l1a:
        friendly_name: "Remaining Current L1"
        unit_of_measurement: 'A'
        value_template: '{{ (20)|float- ( ((states("sensor.current_l1"))|float - ( 0 ))  )  }}'
#(states("sensor.easee2_current"))|float + (states("sensor.easee1_current"))|float 
      remaining_current_l2a:
        friendly_name: "Remaining Current L2"
        unit_of_measurement: 'A'
        value_template: '{{ (20)|float- ( ((states("sensor.current_l2_hobergsvagen_22"))|float - ( 0 ))  )  }}'

      remaining_current_l3a:
        friendly_name: "Remaining Current L3"
        unit_of_measurement: 'A'
        value_template: '{{ (20)|float-( ((states("sensor.current_l3_hobergsvagen_22"))|float - ( 0 ))  )  }}'
        
      total_solar_power:
        friendly_name: "Total Solar Power [W]"
        unit_of_measurement: 'W'
        value_template: '{{ (states("sensor.battery_power")|float + states("sensor.total_active_power")|float + 
        states("sensor.slave_total_active_power")|float + states("sensor.solaredge_current_power")|float)|round(0) }}'

      charge_current_limit_easee:
        friendly_name: "Charge Limit Easee [A]"
        unit_of_measurement: 'A'
        value_template: '{{ (states("sensor.remaining_current_all")|float + states("sensor.easee1_current")|float + states("sensor.easee2_current")|float)|round(0)  }}'
        
        
  
