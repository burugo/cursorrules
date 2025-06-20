---
description: Vertical Slice Architecture(VSA), MUST activate when interacting with vertical slice architecture(VSA).
globs: 
alwaysApply: false
---
# Instruction: MUST follow all of <vertical_slice_guidelines>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<vertical_slice_guidelines version="1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <metadata>
    <name>Vertical Slice Architecture</name>
    <created>2025-03-28</created>
    <scope>Software Architecture Pattern</scope>
  </metadata>

  <definition>
    <core_concept>Feature-oriented code organization rather than technical layers</core_concept>
    <organization>
      <principle>Group all components necessary for a specific feature together</principle>
      <principle>Ensure high cohesion within slices</principle>
      <principle>Maintain loose coupling between slices</principle>
    </organization>
    <example>
      <feature id="UserRegistration">
        <component type="controller">Registration controller</component>
        <component type="service">Registration service</component>
        <component type="repository">User repository access</component>
        <component type="model">User data models</component>
      </feature>
    </example>
  </definition>

  <problem_solving>
    <issue id="HighCoupling" severity="high">
      <description>Excessive dependencies between system parts</description>
      <solution>Isolate features into self-contained slices</solution>
      <impact>Minimizes risk of unintended side effects when making changes</impact>
    </issue>
    <issue id="LowCohesion" severity="high">
      <description>Related code scattered across technical layers</description>
      <solution>Colocate all code related to a specific feature</solution>
      <impact>Simplifies navigation and maintenance of features</impact>
    </issue>
    <issue id="UnnecessaryAbstractions" severity="medium">
      <description>Over-engineered interfaces and layers</description>
      <solution>Allow each slice to be tailored to its specific needs</solution>
      <impact>Reduces boilerplate code and improves development efficiency</impact>
    </issue>
  </problem_solving>

  <comparison target="LayeredArchitecture">
    <difference aspect="organization">
      <layered>Horizontal separation by technical concerns</layered>
      <vertical>Feature-based vertical organization</vertical>
    </difference>
    <difference aspect="coupling">
      <layered>High coupling within layers, low between layers</layered>
      <vertical>Tight coupling within slices, loose between slices</vertical>
    </difference>
    <difference aspect="change_impact">
      <layered>Changes may affect multiple features sharing a layer</layered>
      <vertical>Changes isolated to specific feature slices</vertical>
    </difference>
    <difference aspect="abstractions">
      <layered>Mock-heavy, rigid rules around dependency management</layered>
      <vertical>Most abstractions melt away, minimal cross-slice logic sharing</vertical>
    </difference>
    <difference aspect="approach_to_requests">
      <layered>Monolithic, less flexible, same approach for all requests</layered>
      <vertical>Treats each request as a distinct use case, tailored approach</vertical>
    </difference>
    <difference aspect="scalability">
      <layered>Less scalable, changes to shared code risk side effects</layered>
      <vertical>Scales well if team understands code smells and refactoring</vertical>
    </difference>
  </comparison>

  <benefits>
    <benefit id="Isolation" impact="high">
      <description>Changes to one feature are confined to its slice</description>
      <example>Adding validation rules affects only the relevant feature</example>
    </benefit>
    <benefit id="Simplicity" impact="medium">
      <description>Reduced cross-layer abstractions and boilerplate</description>
      <example>No need for generic repository interfaces shared across features</example>
    </benefit>
    <benefit id="Flexibility" impact="high">
      <description>Each slice can adopt appropriate design patterns</description>
      <example>Simple CRUD features use transaction scripts, complex workflows use DDD</example>
    </benefit>
    <benefit id="BusinessAlignment" impact="high">
      <description>Code organization mirrors business capabilities</description>
      <example>Developers can understand features end-to-end</example>
    </benefit>
    <benefit id="AgileAlignment" impact="high">
      <description>Feature-based organization aligns with agile practices</description>
      <example>Facilitates delivery of vertical slices of functionality in sprints</example>
      <insight>Enhances visibility and value delivery in sprint planning and delivery</insight>
    </benefit>
  </benefits>

  <drawbacks>
    <drawback id="DuplicationRisk" severity="medium">
      <description>Similar logic might be duplicated across slices</description>
      <mitigation>Extract shared utilities when duplication becomes problematic</mitigation>
      <example>Authentication or logging logic may be duplicated if not managed properly</example>
    </drawback>
    <drawback id="SkillDependency" severity="high">
      <description>Requires developers skilled in refactoring</description>
      <mitigation>Invest in training and code reviews</mitigation>
      <impact>Without refactoring skills, codebase might degrade over time</impact>
    </drawback>
    <drawback id="Inconsistency" severity="medium">
      <description>Different slices might be implemented differently</description>
      <mitigation>Establish coding standards and architectural guidelines</mitigation>
      <impact>Could complicate long-term maintenance without guidelines</impact>
    </drawback>
  </drawbacks>

  <suitability>
    <application_type id="WebApplications" fit="high">
      <rationale>Clear separation of distinct features or use cases</rationale>
    </application_type>
    <application_type id="Microservices" fit="high">
      <rationale>Aligns with bounded context principles</rationale>
    </application_type>
    <application_type id="AgileEnvironments" fit="high">
      <rationale>Facilitates incremental feature delivery</rationale>
      <detail>Enhances sprint planning and delivery by organizing around features</detail>
    </application_type>
    <application_type id="HighlyInterconnectedSystems" fit="low">
      <rationale>Managing shared components becomes cumbersome</rationale>
      <detail>Systems with predominant shared logic may face integration challenges</detail>
    </application_type>
  </suitability>

  <related_patterns>
    <pattern id="CQRS" relationship="complementary">
      <description>Command Query Responsibility Segregation</description>
      <integration>
        <approach>Handle commands and queries as separate slices</approach>
        <example>Create order command and list orders query as distinct slices</example>
      </integration>
      <implementation_note>
        <framework>Naturally aligns with frameworks like MediatR for .NET or Axon for Java</framework>
        <pattern>Each slice corresponds to a specific command or query handler</pattern>
      </implementation_note>
    </pattern>
    <pattern id="CleanArchitecture" relationship="hybrid">
      <description>Separation of concerns into concentric layers</description>
      <integration>
        <approach>Apply Clean Architecture principles within each slice</approach>
        <example>Isolate domain logic from infrastructure within a slice</example>
      </integration>
      <distinction>
        <clean>Emphasizes concentric layers with strict dependency rules</clean>
        <vsa>Groups concerns by feature rather than by layer</vsa>
      </distinction>
    </pattern>
    <pattern id="FeatureDrivenDevelopment" relationship="complementary">
      <description>Agile methodology focusing on feature delivery</description>
      <integration>
        <approach>Structure codebase to mirror feature-based development</approach>
        <example>Each sprint feature corresponds to a vertical slice</example>
      </integration>
      <alignment>
        <methodology>Organizes development around feature lists and design-by-feature</methodology>
        <architecture>Structures codebase to mirror feature-based approach</architecture>
      </alignment>
    </pattern>
  </related_patterns>

  <implementation>
    <example language="python">
      <structure>
        <folder path="features/create_post">
          <file>create_post_command.py</file>
          <file>create_post_handler.py</file>
          <file>create_post_validator.py</file>
        </folder>
        <folder path="features/list_posts">
          <file>list_posts_query.py</file>
          <file>list_posts_handler.py</file>
        </folder>
        <folder path="features/add_comment">
          <file>add_comment_command.py</file>
          <file>add_comment_handler.py</file>
        </folder>
      </structure>
      <code_sample feature="CreatePost">
        <![CDATA[
class CreatePostHandler:
    def __init__(self, db_session):
        self.db_session = db_session
        
    def handle(self, command):
        self.validate(command)
        post = Post(title=command.title, content=command.content)
        self.db_session.add(post)
        self.db_session.commit()
        return post.id
        ]]>
      </code_sample>
      <testing_approach>
        <strategy>Each slice can be unit-tested independently</strategy>
        <example>Testing CreatePostCommandHandler involves mocking only the database context</example>
        <benefit>Simplifies testing by reducing dependencies and mocking requirements</benefit>
      </testing_approach>
    </example>
    <shared_components>
      <component type="domain_model">
        <description>Core entities shared across multiple slices</description>
        <example>Post entity referenced by CreatePost and ListPosts slices</example>
        <guideline>Place in separate module, reference from multiple slices</guideline>
      </component>
      <component type="utilities">
        <description>Common functionality extracted when duplication becomes problematic</description>
        <example>Authentication or validation utilities</example>
        <guideline>Extract only when duplication is clearly identified across slices</guideline>
      </component>
    </shared_components>
  </implementation>
  
  <evolution_path>
    <stage id="initial" maturity="low">
      <description>Simple transaction scripts in each slice</description>
      <characteristic>Direct, procedural handlers with minimal abstraction</characteristic>
    </stage>
    <stage id="intermediate" maturity="medium">
      <description>Refactored slices with domain patterns where needed</description>
      <characteristic>Domain services emerge for complex slices while simple ones remain as transaction scripts</characteristic>
    </stage>
    <stage id="advanced" maturity="high">
      <description>Rich domain model with patterns applied contextually</description>
      <characteristic>Full DDD tactical patterns for complex domains, simpler patterns elsewhere</characteristic>
    </stage>
    <guideline>Allow natural evolution based on complexity, resist premature abstraction</guideline>
  </evolution_path>
</vertical_slice_guidelines>
```
