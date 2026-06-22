# How does dbt State calculate that a model has changed?

dbt State only considers substantial changes to a model. Because dbt State understands the entire lineage of your models, it can see through things like whitespace and aliases to determine whether a model is the same or different across environments.
