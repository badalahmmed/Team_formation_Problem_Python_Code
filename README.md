# Team Formation Problem

## Problem Statement:
Given a set of individuals, each with certain skills and preferences, the task is to form teams that satisfy a set of predefined constraints and maximize an objective function. Each team must consist of a specific number of members, and the goal is to optimize either the skills within the teams, the satisfaction of team members' preferences, or some other criteria, subject to the constraints.

## Inputs:

1. **Individuals**: A set of individuals \(I = \{i_1, i_2, ..., i_n\}\), where each individual has:
   - A set of skills \(S_i\), such as programming, communication, design, etc.
   - A preference score \(P_i\) for working with certain individuals.
   - A constraint on the maximum number of teams \(T_i\) they can be assigned to.

2. **Team Requirements**:
   - The total number of teams \(k\).
   - Each team must have exactly \(m\) members.
   - A constraint on the minimum and maximum number of each skill in a team (e.g., each team should have at least 1 programmer but no more than 2 designers).

3. **Skills Matrix**: A matrix \(A\) where each element \(A(i,j)\) represents how well individual \(i\) matches skill \(j\) (e.g., 0 for no skill, 1 for basic knowledge, 2 for intermediate, and 3 for expert).

4. **Preferences Matrix**: A matrix \(P\) where each element \(P(i,j)\) represents how well individual \(i\) would like to work with individual \(j\) (e.g., from 0 to 5, where 0 is neutral and 5 is highly preferred).

## Objective:
The objective is to maximize a function \(F\) that combines:
- The overall skills in each team (e.g., sum of the individual skill levels within a team).
- The satisfaction based on preferences (e.g., sum of the preferences between individuals within a team).
- Any other relevant factors, such as fairness or diversity in skills.

Formally, the objective can be expressed as:

\[
F(T_1, T_2, ..., T_k) = \sum_{t \in \{T_1, T_2, ..., T_k\}} (S(t) + P(t))
\]

Where \(S(t)\) is the total skill score of team \(t\), and \(P(t)\) is the total preference score of team \(t\).

## Constraints:
1. **Team Size**: Each team must consist of exactly \(m\) members.
2. **Skill Requirements**: Each team must satisfy the skill requirements, e.g., each team must have at least one member skilled in programming, at least one in design, etc.
3. **Preference Constraints**: Members of a team must have high preference scores with each other based on the preferences matrix.
4. **Skill Distribution**: The total skills in each team must be balanced according to the predefined skill distribution.
5. **Team Assignment Constraints**: Each individual can only be assigned to a maximum of \(T_i\) teams.
6. **Fairness**: Ensure that no individual is assigned to too many high-preference teams or too many low-skill teams.

## Example:

Suppose there are 6 individuals with the following skills and preferences:

| Individual | Programming | Design | Communication | Preference for Teamwork |
|------------|-------------|--------|---------------|-------------------------|
| i1         | 3           | 0      | 2             | 5 (wants to work with i2) |
| i2         | 2           | 3      | 1             | 4 (wants to work with i1) |
| i3         | 2           | 2      | 3             | 3 (neutral to i1, i2)    |
| i4         | 1           | 1      | 3             | 5 (wants to work with i3) |
| i5         | 2           | 3      | 1             | 2 (neutral)             |
| i6         | 3           | 2      | 1             | 4 (neutral)             |

You want to form 2 teams, each with 3 members. The teams should be as balanced as possible, both in terms of skills and preferences. Additionally, each individual should work with people they prefer, but not necessarily with everyone they prefer.

## Solution Approach:
- **Metaheuristic Algorithms** such as Genetic Algorithms or Simulated Annealing, which explore different configurations to optimize the objective.
# Team Formation Problem

This repository contains an implementation of a **Team Formation Problem** using optimization techniques. The goal of the problem is to create balanced teams of individuals, each with specific skills and preferences. The solution optimizes team assignments based on skills and costs, while ensuring that required skills are met.

## Problem Overview

Given a set of individuals with different skills and preferences, the task is to assign them into teams. Each team must fulfill specific skill requirements, and the overall team assignments must minimize the costs associated with team formation.

### Key Steps:
1. **Load Skills Data**: Read in skills and skills connections from a file.
2. **Load Cost Data**: Read in the cost of forming connections between individuals.
3. **Load Required Skills**: Load the required skills for team formation and match them to the available skills.
4. **Generate Population**: Create a population of potential team configurations.
5. **Optimization**: Optimize the teams based on skills, preferences, and costs.
# Team Formation Problem
This repository contains an implementation of a **Team Formation Problem** using optimization techniques. The goal of the problem is to create balanced teams of individuals, each with specific skills and preferences. The solution optimizes team assignments based on skills and costs, while ensuring that required skills are met.
## Problem Overview
Given a set of individuals with different skills and preferences, the task is to assign them into teams. Each team must fulfill specific skill requirements, and the overall team assignments must minimize the costs associated with team formation.
### Key Steps:
1. **Load Skills Data**: Read in skills and skills connections from a file.
2. **Load Cost Data**: Read in the cost of forming connections between individuals.
3. **Load Required Skills**: Load the required skills for team formation and match them to the available skills.
4. **Generate Population**: Create a population of potential team configurations.
5. **Optimization**: Optimize the teams based on skills, preferences, and costs.
## Setup and Installation
Before running the code, ensure you have the following libraries installed:
pip install numpy

The script is designed to run on Google Colab, and it uses files from Google Drive to load data.

# Data Files

The following data files are required: &#45; @ExtractedSkill_Staff_Expertise_Data.txt@: Contains skills data. &#45; @ConnectionCost_Staff_Expertise_Data.txt@: Contains connection cost data between individuals. &#45; @Required skills 20_1.txt@: Contains required skills for team formation.

Ensure the files are uploaded to your Google Drive and referenced correctly in the script.

# Functions Overview

#1. *Loading Skills Data*

```python

def load_skills_in_memory(skills_file):
    with open(skills_file, "r") as myfile:
        data = myfile.read().splitlines()
    return data

This function loads the skills data from a file into memory.

# 2. *Processing Skills Data*

```python
def process_skills_connection(skills_Data):
    persons_list=[];
    skills_list=[]
    unprocess_skills_connections=[];
    for idx in skills_Data:
        content = idx;
        process_already=False;
        results= content.split("=");
        for result in results:
            if result.strip() == "Total Persons".strip():
                total_person = int(results[1]);
                process_already=True;
            elif result.strip() == "Total Skills".strip():
                total_skills =int (results[1]);
                process_already=True;
            elif result.strip() == "Persons Mapping".strip():
                persons= results[1].split(",");
                for person in persons:
                    ## need to create a string array
                    persons_list.append(person);
                process_already=True;
            elif result.strip() == "Skills Mapping".strip() :
                skills= results[1].split(",");
                for skill in skills:
                    ## need to create a string array
                    skills_list.append(skill);
                process_already=True;
            else:
                if  content not in unprocess_skills_connections and process_already == False :
                    unprocess_skills_connections.append(content);
    # Process the skills connection
    skills_connections = [[0] * (total_skills) for _ in range(total_person)]
    # initialize initial connections
    i = 0
    while (i < len(unprocess_skills_connections)) :
        s = unprocess_skills_connections[i].strip()
        result = s.split("=")
        val = result[1].split(":")
        j = 0
        while (j < len(val)) :
            skills_connections[i][j] = int(int(val[j]))
            j += 1
        i += 1
    return [total_person, total_skills, persons_list, skills_list, skills_connections];


This function processes the skills data into a structured matrix that can be used to evaluate team assignments.

# 3. *Processing Cost Data*

bc(python). 
def process_Connection_cost(cost_data, total_persons):
    unprocess_costs_connections=[];
    for data in cost_data:
        process_already= False;
        results= data.split('=');
        for result in results:
            str_value= result;
            if str_value.strip() == "Persons Mapping".strip():
                process_already = True;
                continue;
            else:
                if  data not in unprocess_costs_connections and process_already == False :
                    unprocess_costs_connections.append(data);
    # Process the connection cost
    connections_cost = [[0.0] * (total_persons) for _ in range(total_persons)]
    i = 0
    while (i < len(unprocess_costs_connections)) :
        s = unprocess_costs_connections[i].strip()
        result = s.split("=")
        val = result[1].split(":")
        j = 0
        while (j < len(val)) :
            connections_cost[i][j] = float(val[j]);
            j += 1
        i += 1
    return connections_cost;


This function processes the cost data and creates a cost matrix to calculate the cost of forming teams.

# 4. *Generating Random Sequences*

bc(python). 
def generate_random_sequence(dimention):
    ar = [0] * (dimention)
    d = 0
    tmp = 0
    generator =  random;
    dummy_solution_array = [0] * (dimention)
    counter = 0
    while (counter < dimention) :
        dummy_solution_array[counter] = counter
        counter += 1
    # copy array from seq_arr
    ar = dummy_solution_array;
    # swap with new ar with random index
    # the first index and last index must not change
    i = 0
    while (i < dimention - 1) :
        d = i + (generator.randint(0, (dimention - 1 - i)));
        # generate random number between i and (dim-1-i)
        tmp = ar[i]
        ar[i] = ar[d]
        ar[d] = tmp
        i += 1
    return ar


This function creates a random sequence to initialize the population for optimization.

# 5. *Objective Function*

bc(python).
def objective_function(arr, skills_to_find):
    total_skills=len(skills_to_find);
    consist_skills_value = False
    trim_seq =  []
    clone_skills_to_find = skills_to_find ;
    for i in range(len(arr)):
        consist_skills_value = False
        if (i == 0) :
            person= arr[i];
            update_answer = skills_connections[person]
            consist_skills_value = contain_skills_to_find(update_answer, clone_skills_to_find);
            if (consist_skills_value== True) :
                trim_seq.append(arr[i])
        else :
            person= arr[i];
            tmp_answer=skills_connections[arr[i]]
            consist_skills_value = contain_skills_to_find(tmp_answer, clone_skills_to_find)
            if (consist_skills_value==True) :
                trim_seq.append(arr[i])
                update_answer = merge_elements_(update_answer, tmp_answer)
                if (complete_merge_sequence_(update_answer, skills_to_find)) :
                    break;
    return trim_seq;


This function evaluates how well a team configuration meets the required skills and preferences.

# Jaya Algorithm

The main optimization algorithm used is *Jaya Algorithm*, which aims to minimize the objective function by iterating over possible solutions and adjusting them to move towards better solutions.

# 1. *Population Initialization*

bc(python). 
def initialize_population(task):
    population = [[0] * (total_person) for _ in range(population_size)]
    # Generate Random Population and copy it into population variable
    arr = None
    row = 0
    while (row < population_size) :
        arr = generate_random_sequence(total_person)
        population[row] = arr
        task_1_fitness = objective_function(arr, task.copy())
        task1_obj_person = objective_value1_(task_1_fitness)
        task1_obj_costs = objective_value2_(task_1_fitness, connections_cost)
        row+=1


This function initializes a population of possible solutions, where each individual in the population represents a possible team configuration.

# 2. *Fitness Calculation*

bc(python). 
def create_rank_fitness(fitness):
    population_size= len(fitness);
    delta = 1.0E-4
    clone_fitness = fitness
    rank = [0] * (population_size)
    clone_fitness.sort()
    idx = 0
    i = 0
    while (i < len(fitness)) :
        j = 0
        while (j < len(fitness)) :
            if (abs(fitness[i] - clone_fitness[j]) < delta) :
                rank[idx] = j + 1
                clone_fitness[j] = sys.maxsize
                idx += 1
                break
            j += 1
        i += 1
    return rank


This function ranks the fitness values of the population to select the best solutions.

# Running the Algorithm

To run the team formation problem:

# Load the necessary data files.
# Initialize the population of potential team configurations.
# Apply the optimization algorithm to improve team assignments.
# Evaluate the solution based on the objective function.

# Main program controller
skills_file = &quot;/content/drive/MyDrive/Team Formation/ExtractedSkill_Staff_Expertise_Data.txt&quot;
skills_data = load_skills_in_memory(skills_file)

# Process skills and cost data
total_person, total_skills, persons_list, skills_list, skills_connections = process_skills_connection(skills_data)

cost_file = '/content/drive/MyDrive/Team Formation/ConnectionCost_Staff_Expertise_Data.txt'
cost_data = load_cost_in_memory(cost_file)
connections_cost = process_Connection_cost(cost_data, total_person)

# Load required skills
skills1 = load_skills_to_find(&quot;/content/drive/MyDrive/Team Formation/Required skills 20_1.txt&quot;, skills_list)

# Initialize population and start optimization
population_size = 100
initialize_population(skills1.copy())



# Conclusion
This repository implements an optimization approach to the team formation problem. By using a combination of skill&#45;based and cost&#45;based optimization techniques, it aims to form the best possible teams based on predefined criteria.

This Markdown file includes the full Python code as well as explanations for each section. You can copy and paste this directly into a @README.md@ file for your GitHub repository.
