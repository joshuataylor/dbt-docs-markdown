# Why doesn’t an indirectly referenced upstream public model appear in Explorer?

For [project dependencies](https://docs.getdbt.com/docs/mesh/govern/project-dependencies.md) in Mesh, [Catalog](https://docs.getdbt.com/docs/explore/explore-multiple-projects.md) only displays directly referenced [public models](https://docs.getdbt.com/docs/mesh/govern/model-access.md) from upstream projects, even if an upstream model indirectly depends on another public model.

So for example, if:

* `project_b` adds `project_a` as a dependency
* `project_b`'s model `downstream_c` references `project_a.upstream_b`
* `project_a.upstream_b` references another public model, `project_a.upstream_a`

Then:

* In Explorer, only directly referenced public models (`upstream_b` in this case) appear.
* In the [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md) lineage view, however, `upstream_a` (the indirect dependency) *will* appear because dbt dynamically resolves the full dependency graph.

This behavior makes sure that Catalog only shows the immediate dependencies available to that specific project.
