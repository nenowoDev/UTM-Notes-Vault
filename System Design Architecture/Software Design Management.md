- transform functional & non functional requirements into models

Quality 
- relative to stakeholders ( may be different for users/dev etc. )
- cost and scheduling is evaluated from management side of things


Design management framework
- view the design phase as project
- design project lifecycle below
- initial(small contribution) , implementation(high) , termination(small)
- initial
	- develop plan/clear understand of task/resource needed
- implementation
	- deliverables/more effort/highest % 
- termination
	- verify everything for smooth transition to coding
- performance measure for design project lifecycle
	- planning stage
		- thoroughness
		- completeness
		- accuracy of estimate
	- implementation
		- quality
		- managing to monitor plan is followed



# Planning Stage
- scoping (identify task/budget)
	- work breakdown structure (wbs)
		- steps
			- begin with statement of work (SOW) (aka set of main objective)
			- decompose it into task
			- decomposes into subtask
			- decomposes into work packages
		- evaluating wbs
			- scope accuracy
				- overall objective and nothing else
				- no information about task relationship/duration/skill required ok
				- represent only activity that consume resource/time/effort
			- completeness
			- level of details
			- WBS table
				- Outline ( 1.1 , 1.1.2.1)
				- Task Name
	- budgeting
		- direct cost (can directly be tied to development)(labor/equipment)
		- indirect cost (admin expenses)
- organizing (after wbs ) (conduct task/est duration/est predecessors)
	- linear responsibility chart
		- identify responsible people
		- Table
			- Outline
			- Task Name
			- Staff 1
			- Staff 2
			- Staff n 
			- in cells of Staff 1-n ,, fill 1-3 (Primary responsibiliy, Support role, review role..)
	- scheduling (cannot delay this, since project cannot start without this)
		- Gantt Charts
			- Task
			- Duration
			- Predecessors
		- Network Diagram
			- Same as Gantt charts but like edges and nodes
			- ES EF LS LF (earliest start/finish , latest ..)
			- Critical path (longest path)
			- critical activities is things on critical path
			- slack time for non critical activities only (can slack without affecting project duration)
			- earliest/latest completion time
			- slack times
- establish change control policy
	- submit change req
	- they eval based on merit/complexity/side effects
	- based on eval , assign change priority
	- dev formal review , update est budget , schedule all things
	- get customer approval

# Implementation Stage 
- management tool , 
	- Gantt chart
	- Earned value management (evm)
		- determine progress of task based on value of work vs work that was expected to complete at that time
		- EV(earned value) = % work completed * planned cost for work
		- guideline for estimate
			- 50-50 (50 when start , 100 when finish)
			- (0-100)
			- critical input use (% crit input used vs overall expected to be used)
			- proportionality (% proportion of actual time/cost versus planned)


| Term | Full                       | Alt  | Full                         | Formula                                 |
| ---- | -------------------------- | ---- | ---------------------------- | --------------------------------------- |
| EV   | Earned Value               | BCWP | Budgeted cost work performed | % work completed * planned cost of work |
| AC   | Actual cost                | ACWP | Actual cost work performed   |                                         |
| PV   | Planned value              | BCWS | Budgeted cost work scheduled |                                         |
| CV   | Cost variance              |      |                              | EV-AC                                   |
| SV   | Schedule Variance          |      |                              | EV-PV                                   |
| CPI  | Cost Performance Index     |      | > 1 over budget              | EV/AC                                   |
| SPI  | Schedule Performance Index |      | >1 ahead schedule            | EV/PV                                   |
| BAC  | Budget @ completion        |      |                              |                                         |
| ETC  | Estimate time complete     |      |                              |                                         |
| EAC  | Estimate Actual cost       |      |                              | ETC+AC                                  |

Sara is managing a software design project composed of 10 tasks.  Each task was estimated to cost $1,000; therefore, BAC = $10,000.  Each task was estimated to be completed in a month; therefore, the total planned duration for the project was estimated to be 10 months.  Since each task is expected to have the same cost and duration, the completion of an individual task represents 10% completion of the design project.  In the current fifth month, only three tasks have been fully completed at a cost of $4,000.  Sara needs to provide upper management with current progress status.

| Term | Formula                                                    | Calculation                |
| ---- | ---------------------------------------------------------- | -------------------------- |
| EV   | %work completed * planned cost                             | 0.3* 10,000 = 3,000        |
| AC   | given                                                      | 4,000                      |
| PV   | given ( at 5 month , planned 5 task or 5,000 is completed) | 5,000                      |
| CV   | EV-AC                                                      | 3,000-4,000 = -1,000       |
| SV   | EV-PV                                                      | 3,000-5,000=-2,000         |
| CPI  | EV/AC                                                      | 3,000/4,000 = 0.75         |
| SVI  | EV/PV                                                      | 3,000/5,000=0.6            |
| BAC  | given                                                      | 10,000                     |
| ETC  | (BAC-EV)/CPI                                               | (10,000-3,000)/0.75 =9,333 |
| EAC  | ETC+AC                                                     | 9,333+4,000                |

So basically have 4 categories
EV,AC,PV = calc

CV,SV  = EV - (AC|PV)

CPI,SPI = EV / (AC|PV)

BAC,ETC,EAC = BAC-EV/CPI + AC



# Termination Stage
- verification
- make sure latest version
- update schedule/cost
- re evaluate budget 
- communicate result to management