Web-based UI
Features:
1. Add plant model
2. Performance Logic
## Plant Model
Features:
1. Tree Structure
2. How to store
Each plant model has module add ons:
- factory
- line
- cell
Each cell can attach a performance module


Plant
	Area: Shop floor represents a building
			Line: Represents one product
				Unit: group of equipment that has a function (evaporator, dryer, etc)
					Machine: bottom layer (homogenizer, evaporator) <-- automation is here


Find out how to query the SQL table

### Indicator List
total_duration
total_event_count
unplanned_count
unplanned_duration
planned_count
planned_duration
expected_planned_duration
expected_duration
not_occupied_duration
running_duration
breakdown_count
breakdown_duration
process_failure_count
process_failure_duration
minor_stop_count
minor_stop_duration
unknown_count
unknown_duration
nominal_speed
maintenance_count
maintenance_duration
capex_investment_count
capex_investment_duration
changeover_group_count
changeover_group_duration
cleaning_count
cleaning_duration
legal_restriction_count
legal_restriction_duration
line_related_changeover_count
line_related_changeover_duration
meals_or_breaks_count
meals_or_breaks_duration
machine_process_waiting_count
machine_process_waiting_duration
no_demand_count
no_demand_duration
operational_stoppages_count
operational_stoppages_duration
product_finished_early_count
product_finished_early_duration
product_related_changeover_count
product_related_changeover_duration
production_plan_finished_earlier_count
production_plan_finished_earlier_duration
rework_count
rework_duration
speed_loss_count
speed_loss_duration
startup_shutdown_count
startup_shutdown_duration
training_or_meeting_count
training_or_meeting_duration
trials_and_tests_count
trials_and_tests_duration
waste_group_count
waste_group_duration
good_count
waste_count
total_count
good_count_po_uom
waste_count_po_uom
total_count_po_uom
waste_rework
speed_loss

Energy indicators:
waterusage_cons_m3
consumption_in_gj
consumption_in_ghg
electricity_cons_gj
steam_cons_ton
steam_cons_gj
fg_cost_tons
fg_production_tons


1 byte status
1 byte fault code
Running - 0
Idle - 1
Not Occupied - 5
Stoppages - 6
Has PO
PO running

Calculation:
Availability = (runtime - stoppages - not occupied) / runtime

Efficiency:
Total Count = Good count + Bad Count <-- PLC Cumulative Sum in SQL
Performance = (Ideal Cycle Time x Total Count ) / run time
Quality:
Quality = Good count / total count

OEE:
OEE = Availability x Performance x Quality

Route Definitions:


Display:
1. Piechart runtime %s
2. Ghantt chart runtime event hist
3. 