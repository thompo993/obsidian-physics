
# Session 1. Chair: George Phillips 
### Tags: 
[[muons]]
[[Neutrons]]
[[crystallography]] 
[[SANS
[[NMSUM]]

#### Crystallography – Jing Ming

beyond average structure, using neutron scattering 

why does atomic arrangement matter? 
- diamond vs graphene, both just carbon
- x-rays interact w electrons 
- periodic atomic arrangement produce diffraction
- brag peaks contain the information 

average structure is not enough
local structure can effect
- conductivity
- magnetism 
- energy storage

Why does XRD miss local information 
average structure does not represent actual structure 
local structure determination is import to understand mechanisms. 

we can probe local structure with neutron PDF. 
pair distribution function can telll us the probability of finding another atom at distance r from a given atom

provides relative positions of atom pairs 

**we need both**
- holistic view from log and short range structures. 

why does local structure matter in a real system? Positions and concentrations of vacancies strong influence . . . 




#### Crystallography Science talk – Kirstin Wilson, ILL, “In Situ and PDF Neutron Diffraction for the Advanced Structural Analysis of MOF Catalysts for Carbon Dioxide Valorisation” 

[[SEM]]
we want do reduce co2 emissions uses is: 
- reverse water gas shift reaction
- catalysists

MOF - metal organic frameworks 
- tunable structure
- very generalizable

RPF-4 MOF synthesis
- rod shaped secondary building units



#### Muons – Alex Louat 


[[superconductivity]]
Introduction to MuSR

Muon spin relaxation/rotation/resolnce 
what is a [[muon]] 

we are interested in posotive muons. 

muons have large gyromagnetic ration 
100% spin polarised 
NMR spin porlaisation is less than 1% 

asymmetric emissions of positrons allows the magnetic composition of the sample to be determined. 

see [[Elements of Muon Spectroscopy]] for more detail

raw data to useful inofrmation calculate asymetry. 
$A(t) = \frac{B(t) - \alpha F(t)}{B(t) + \alpha F(t)}$

##### zero field 
gaussian distribution of a static magnetic field
nuclear dipolar fields 
magnetic materials with disordered spins 
amorphous materials 

static limit **static kubo-toyabe**
the polarisation changes with respect to $\nu/\Delta$


##### longitudinal field 
fast dynamics in longitudinal geometry 

###### Transverse field 
dynamics transverse in the field 
used in studies of superconductors

###### negative muons
capture-cascade-decay 
elemental analysis 
depth scans 


##### key areas
- magnetism 
- dynamics in magnetism 
- dynamic process
- superconductors
- semiconductors and dielectrics
	- defect states 
	- muonium formation 
- chemistry
	- chemical environments 
	- reaction kinetics





#### Muons Science Talk – Tom Brogan, University of Birmingham, “Revisiting the muon response in YbBO3

##### QSL - quantum spin liquids 
- materials that have antiferromagnetic interactions hosting frustration 
- paramagnetic even down to 0K 
- quantum fluctuations rather than thermal fluctuations 
- excitations are fractional carry S=1/2 not S=1 
	- excitations obey Anyon statistics not bosonic or fermionic 
- ground state:
	- exist as a superposition of all possible ground states, like a singlet, but over the whole lattice
	- long range quantum entanglement due to this quantum superposition, not describable as  a product state. 
##### YBO$_{3}$ 
S$_{eff}$ = 1/2 at low temperatures do to field splitting of Yb spins 
two YB sites. 

###### previous characterization
ordering demonstrated via AC susceptibility and two independent heat capacity measurements 
transitions occur at 0.4K 
neutron powder diffraction suggests antiferromagnetic ground state
	this should be seen in MuSR ZF data.
No ordering seen down to 20mk in muon ZF-MuSR data. 
fitted using stretched exponential 
indicated distribution of relaxation rates, common in frustrated systems. 

initial muon used powdered pressed metal
thermalization flagged as an issue 
used with silver (not dopant)

fitting function 
- silver fraction, 50% simple exponential 
- paramagnetic regime unordered fraction, fit at high temperature
- ordered fraction: 
	- 2/3 oscilations: long range order $\omega$ set by local field B 
	- 1/3 powder tail, due to randomly orieted powder
	- 1/3 of muons spins parrallel to internal field 
	- fast term: some fast dynamics

![[fig-fitting-func-NMSUM-260413.jpg]]

other extreme conditions now to be trialed. 

**find and ask him about practical applications!**

- practical applications "quantum ram" future applications, no current direct application, super cool tho


# Session 2. Chair: Rakin Gilani

#### Excitations and Polarisation – George Wood 

- excitations are internal dynamical fluctuations in condensed matter
- allows determination of the Hamiltonian of a given system 
- determine many physical properties in condensed matter 
- crystal field excitations 

Why are neutrons a good probe of excitations? 
	- thermal neutrons are comparable toe excitations and atomic spacing n condensed matter
	- in an inelastic experiment the difference between the momentum and energy of the incoming scattered neutrons provides the momentum and energy of the excitations. 
	- however these events are very rare. 

#### triple axis spectrometers
direct geometry field of flight spectrometers 
using time structure of a pulsed source and a chopper to select very sharp deltas from this pulsed source, which determines the incoming momentum. 
survey very large regions of reciprocal space 
lower flux instruments but have exceptionally low background (good signal to noise)

##### polarisation 
separate particular cross sections. e.g coherent/incoherent and magnetic/nuclear scattering,
allows deduction of complex magnetic structures. 
varied instrumentation

measurements provide stringent and comprehensive tests of model Hamiltonians 
explain physical properties of materials 
due to neutron energies and wavelengths and other properties such as large magnetic interaction, neutrons are an exemplary probe of condensed matter excitations. 

calibration done using vanadium, boring, total inelastic scattering

#### Excitations and Polarisation Science talk – Lewis Giannelli, University of Glasgow / ILL, “Magnetic ground state and excitations in the S=1/2 kagome Heisenberg magnet Averievite.” 

[[magnetic frustration]]
[[quantum spin liquids]]
magnetic frustration 
- competing magnetic interactions lead to frustration 
	- classic example is Ising model on triangle 
	- can lead to complex magnetic ground states

experimental sign of confirming QSL
- magnetometry - lack of magnetic phase transition 
- frustration index - ratio of mean field and local ordering temperatures
- magnetic heat capacity - temperature dependence
- MuSR, flat T independent signal
- spinons -free spins in the lattice 

Averievite I - what this guy studies, what are they? 

growth of crystals [[seed flux growth]]
- chemical vapour transport - good doping, bad size
- flux growth - bigger crystals, poor doping
- structure down to 150K solved

magnetic structure of these crystals are an issue, use MuSR! 

#### Spectroscopy - Sam Lambrick

 - Inelastic neutron scattering (INS)
 -  $Q = |K_{f} - K_{i}|$ $\hbar \omega = \Delta E = E_f - E_i$

Why neutrons for spectroscopy 
- exception hydrogen sensitivity 
- low energy (now damage)
- bulk sensitive, penetrate though mm-cm sized samples 

##### INS vibrations on the TOSCA spectrometer 
- thermal energy neutrons (meV range)
- Neutron loses enegy to excite a vibration

what does it tell us? 
- similar information to IR/Raman but all vibrations are active, no symetry selection rules
- peak intensities are proportional to amplitude of atomic motion (and neutron cross section can be predicted)
	- good for interpreting data and benchmarking theory. 
- Interpretation of INS spectra relies of DFT modelling
- Built into mantid!  

##### Quasi elastic neutron scattering (QENS)
- inelastic and elastic events give well defined peaks 
- if there is atomic motion from less well define dynamics, the scattered neutron picks up  a small energy doppler shifts
- lots of atoms moving stochastically, the sharp elastic line broadens, hence the "quasi elastic"
- width of the elastic line broadening tells you the timescale, and what type of motion
- the intruments used are **IRIS** **OSIRIS** and **LET**

##### Deep inelastic neutron scattering (compton scattering )
- high energy for neutron scattering neurons in the eV range (epithermal)
- mass of nucleus is $\Leftrightarrow$   mean $\hbar \omega$
- neutron mass spec
- done on VESUVIO 

#### Spectroscopy Science Talk – Tahlia Palmer, University of Birmingham, “Understanding the role of temperature in impact sensitivity of energetic materials

- energetic materials
	- pyrotechnics 
	- propellants 
	- explosives 
	- anything Mikey would mishandle
we have a lot of unplanned initiations of EMs
high performing material is typically correlated with high  sensitivity 
we basically drop a steel ball onto the energetic materials to step it 
- how do we transport this 
- how do we synthesis it 
- how do we initiate an unknown explosive

using a new method, sensitivity in silico to predict impact sensitivity, this works alright

now want to use this relative to temperature 
heating fox 7 causes it too decrease in sensitivity, this is an anomaly. 



# Session 3. Chair: Josie Binks 
#### SANS – Lauren Matthews SANS

##### Small angle nuetron scattering
incidnet radiation $\rightarrow$  scattered radition $\rightarrow$ radial averaging
			 $\rightarrow$ transmitted radiation  

##### what can we get from SANS measurements 
- Size 
- interactions 
- shape 
- concentration
- p(r) distribution





#### Science Talk – Sujaya Shrestha, STFC, “Practical Tips for Small-Angle Scattering Data Analysis: Insights from SasView 

###### what is sasview? 
- data analysis for xray and neutron simulation
- open source collaborations 
- different ways to look at data 
	- different designs for specific types of scattering analysis
	- all uses the same data, but each one for a different question
##### Fitting perspective
- you can choose a shape
- choose a model, type of shape
- choose structure factor
- then it  computes a beautiful fit
- slicers 
	- this makes a 1D average of the 2D data
	- multiple slicers all at once 
	- symmetric slicers - useful for Bragg peaks. 
- corfunc - correlation functions 
	- scattering data in reciprocal space
	-  1D data
	- Fourier transform???
	- low and high Q fitting
- invariant perspective 
	- uses the scattering invariant
	- volume fraction of different phases 
- can invert reciprocal space into actual space
- read more about it on https://www.sasview.org/ 
- publications are increasing that have referenced sasview 
- open community of collaborators 
- NIST 
- STFC 
- ESS
- ILL -Neutrons for Society 


#### Development Reflectometry – Oleksandr Tomchuk 
[[Reflectometry]] 
##### types of experiment
- radiography 
- irradiation
- diffraction 
- SANS 
- inelastic scattering

##### neutron reflectometry 
- specular reflection 
- reflectivity curve 
- depth profile 
- 


#### Science Talk – Reflectometry Manmeet Kaur Sodhi, ILL, “Multi scale Mechanics of Microgel-Stabilized Foams: Linking Particle Deformation to Macroscopic Foam Structure”
- microgels are soft deformable particles 
- deformation $\rightarrow$  

know:
- microgels absorb and stabilise foams 
- softness allows strong deformation at interfaces 
- force spectroscopy 

- contact stiffness increases with crosslinking density ($\chi$)
- uses force spectroscopy 
- 






### Session 4. Chair: Abhjith Prasad

#### Imaging and Engineering - Alessandro Tengattini
- tomography - a glimpse of some differences between neutrons x-rays 
- zoom in continuously you get a attenuation coefficient, for each pixel.

what is radiography 
- neutrons are incident on a sample they can be transmitted scattered, absorbed etc. 

attenuation tomography: the [beer lambert law]
$I/I_0 = e^{\mu \rho x}$ 
- mu is the attenuation coefficient 
- rho is the density 
- x is the path length 

collimation and penumbra
collimation - essential concept in neutron imaging, sometimes overlooked in laboratory X-ray scanners (spot size)

##### Imaging and Engineering Science Talk – Isabel Antony, UCL, “Electrolyte Dynamics Observed via Operando Neutron Imaging” 
[[batteries]]

lithium ion batteries degrade over time 
always overcharge batteries when you first get them



#### Disordered Materials – Tom Headen



#### Disordered Materials Science talk – Ivan Klassen, The University of Manchester, “Absorption of gases in nanoconfined liquids studied by total neutron scattering”


