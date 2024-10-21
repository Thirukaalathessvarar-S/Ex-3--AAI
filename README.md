# EX-03 Implementation of Approximate Inference in Bayesian Networks
## Reg no:212222230161
## Name:Thirukaalathessvarar S
### Aim: 
To construct a python program to implement approximate inference using Gibbs Sampling.**DATE :00.00.2024**
### Algorithm:
   Step 1: Bayesian Network Definition and CPDs:
    <ul> <li>Define the Bayesian network structure using the BayesianNetwork class from pgmpy.models.</li>
    <li>Define Conditional Probability Distributions (CPDs) for each variable using the TabularCPD class.</li>
    <li>Add the CPDs to the network.</li></ul>
    Step 2: Printing Bayesian Network Structure:
    <ul><li>Print the structure of the Bayesian network using the print(network) statement.</li></ul>
   Step 3: Graph Visualization:
    <ul><li>Import the necessary libraries (networkx and matplotlib).</li>
    <li>Create a directed graph using networkx.DiGraph().</li>
    <li>Define the nodes and edges of the graph.</li>
    <li>Add nodes and edges to the graph.</li>
    <li>Optionally, define positions for the nodes.</li>
    <li>Use nx.draw() to visualize the graph using matplotlib.</li></ul>
    Step 4: Gibbs Sampling and MCMC:<br>
    <ul><li>Initialize Gibbs Sampling for MCMC using the GibbsSampling class and provide the Bayesian network.</li>
    <li>Set the number of samples to be generated using num_samples.</li></ul>
    Step 5: Perform MCMC Sampling:<br>
    <ul><li>Use the sample() method of the GibbsSampling instance to perform MCMC sampling.</li>
    <li>Store the generated samples in the samples variable.</li></ul>
    Step 6: Approximate Probability Calculation:<br>
    <ul><li>Specify the variable for which you want to calculate the approximate probabilities (query_variable).</li>
    <li>Use .value_counts(normalize=True) on the samples of the query_variable to calculate approximate probabilities.</li></ul>
    Step 7:Print Approximate Probabilities:<br>
    <ul><li>Print the calculated approximate probabilities for the specified query_variable.</li></ul>
### Program:
##### IMPORTING LIBRARIES
```Python
from pgmpy.models import BayesianNetwork
from pgmpy.inference import VariableElimination
from pgmpy.factors.discrete import TabularCPD
```
##### DEFINING MODEL STRUCTURE
```Python 
alarm_model = BayesianNetwork([("Burglary", "Alarm"),("Earthquake", "Alarm"),
                               ("Alarm", "JohnCalls"),("Alarm", "MaryCalls"),])
```
<BR>

##### DEFINING PARAMETERS USING CPT AND ASSOCIATING WITH MODEL
```Python
cpd_burglary=TabularCPD(variable="Burglary", variable_card=2,values=[[0.999], [0.001]])
cpd_earthquake=TabularCPD(variable="Earthquake", variable_card=2,values=[[0.998], [0.002]])
cpd_alarm=TabularCPD(variable="Alarm",variable_card=2,
                       values=[[0.999,0.71,0.06,0.05],[0.001,0.29,0.94,0.95]],
                       evidence=["Burglary","Earthquake"],evidence_card=[2,2],)
cpd_johncalls=TabularCPD(variable="JohnCalls",variable_card=2,values=[[0.95, 0.1], [0.05, 0.9]],
                           evidence=["Alarm"],evidence_card=[2],)
cpd_marycalls=TabularCPD(variable="MaryCalls",variable_card=2,values=[[0.99, 0.3], [0.01, 0.7]],
                           evidence=["Alarm"],evidence_card=[2],)
alarm_model.add_cpds(cpd_burglary, cpd_earthquake, cpd_alarm,cpd_johncalls, cpd_marycalls)
```
<BR>

##### CREATE A DIRECTED GRAPH,ADDING NODES AND EDGES,SETTING POSITION AND DISPLAY GRAPH
```Python
G=nx.DiGraph()
nodes=['Burglary','Earthquake','JohnCalls','MaryCalls']
edges=[('Burglary','Alarm'),('Earthquake','Alarm'),('Alarm','JohnCalls'),('Alarm','MaryCalls')]
G.add_nodes_from(nodes)
G.add_edges_from(edges)
pos={'Burglary':(0,0),'Earthquake':(2,0),'Alarm':(1,-2),
    'JohnCalls':(0,-4),'MaryCalls':(2,-4)}
nx.draw(G,pos,with_labels=True,node_size=900,node_color="PINK",font_size=10,
        font_weight="bold",arrowsize=20)
plt.title("Bayesian Network: Burglar Alarm Problem")
plt.show()
```
<BR>

##### INITIALIZE GIBBS SAMPLING FOR MCMC
```Python
gibbssampler=GibbsSampling(alarm_model)
samples=gibbssampler.sample(size=10000)
query_variable="Burglary"
query_result=samples[query_variable].value_counts(normalize=True)
print("\n Approximate probabilities of {}:".format(query_variable))
print(query_result)
```
<BR>
  
### Output:
<img valign=top src="https://github.com/user-attachments/assets/efa897c1-9a05-4516-8d27-bd281fac4f05">
<img valign=top src="https://github.com/user-attachments/assets/c62c6f5c-e219-4412-b990-69222e829dfa">

### Result:
Thus, Gibb's Sampling( Approximate Inference method) is succuessfully implemented using python.
