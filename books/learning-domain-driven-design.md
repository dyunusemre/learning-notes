# **Chapter 1. Analyzing Business Domains**

# **Chapter 2. Discovering Domain Knowledge**

1. ## Business Problems
   
   1. The software systems we are building are solutions to business problems
   2. Business problems appear both at the business domain and subdomain levels.
        1. Subdomains are finer-grained problem domains whose goal is to provide solutions for specific business capabilities.
        2. A knowledge management subdomain optimizes the process of storing and retrieving information
2. ## Knowledge Discovery
    1. To design an effective software solution, we have to grasp at least the basic knowledge of the business domain.
    2. To be effective, the software has to mimic the domain experts’ way of thinking about the problem
    3. **As Alberto Brandolini1 says, software development is a learning process; working code is a side effect.**
    4. A software project’s success depends on the effectiveness of knowledge sharing between domain experts and software engineers.
    5. Effective knowledge sharing between domain experts and software engineers requires effective communication.
3. ## Communication
    1. software projects require the collaboration of stakeholders in different roles: domain experts, product owners, engineers, UI and UX designers, project managers, testers, analysts, and others
    2. ![Alt text](image.png)
    3. In any translation, information is lost; in this case, domain knowledge that is essential for solving business problems gets lost on its way to the software engineers. 
    4. Domain-driven design proposes a better way to get the knowledge from domain experts to software engineers: by using a ubiquitous language.
4. ## What Is a Ubiquitous Language?
    1. Using a ubiquitous language is the cornerstone practice of domain-driven design.
    2. if parties need to communicate efficiently, instead of relying on translations, they have to speak the same language.
    3. The traditional software development lifecycle implies the following translations:
        - Domain knowledge into an analysis model
        - Analysis model into requirements
        - Requirements into system design
        - System design into source code
    4. Instead of continuously translating domain knowledge, domain-driven design calls for cultivating a single language for describing the business domain: the ubiquitous language.
    5. All project-related stakeholders—software engineers, product owners, domain experts, UI/UX designers—should use the ubiquitous language when describing the business domain.
5. ## Language of the Business
    1. It’s crucial to emphasize that the ubiquitous language is the language of the business
    2. As such, it should consist of business domain–related terms only. No technical jargon!
        1. ### Consistency
            1. The ubiquitous language must be precise and consistent.
            2. each term of the ubiquitous language should have one and only one meaning
                1. *Ambiguous terms*
                    - Ubiquitous language demands a single meaning for each term, so “policy” should be modeled explicitly using the two terms regulatory rule and insurance contract.
                2. *Synonymous terms*
                    - Two terms cannot be used interchangeably in a ubiquitous language.
6. ## Model of the Business Domain
    1. ### What Is a Model?
        1. A model is not a copy of the real world but a human construct that helps us make sense of real-world systems.
    2. ### Effective Modeling
        1. All models have a purpose, and an effective model contains only the details needed to fulfill its purpose.
        2. a useful model is not a copy of the real world. Instead, a model is intended to solve a problem, and it should provide just enough information for that purpose.
        3. In its essence, a model is an abstraction
            1. The notion of abstraction allows us to handle complexity by omitting unnecessary details and leaving only what’s needed for solving the problem at hand.
    3. ### Modeling the Business Domain
        1. 






