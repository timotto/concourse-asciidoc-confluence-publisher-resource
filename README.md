# Concourse Asciidoc Confluence Publisher Resource

A Concourse resource that uses a set of libraries and plugins that allows the publication of asciidoc documentation to Atlassian Confluence made possible by the [Confluence Publisher](https://github.com/alainsahli/confluence-publisher) Maven plugin.

All requirements of Confluence Publisher apply, eg the directory structure of asciidoc files.

## Installing

Add the resource type to your pipeline:

```yaml
resource_types:
- name: asciidoc-sink
  type: docker-image
  source:
    repository: timotto/concourse-asciidoc-confluence-resource
```

## Source Configuration

* `url`: *Required.* Root URL of Confluence.
* `username`: *Required.* Username with write permissions.
* `password`: *Required.* Password for user.
* `space`: *Required.* Space Key.
* `ancestor`: *Required.* ID of the ancestor page.

## Behavior

### `check`: Not supported.

### `in`: Not supported.

### `out`: Run Confluence Publisher to publish asciidoc on Confluence.

## Example

### Upload generated documentation

Define the resource:

```yaml
resources:
- name: generated-report
  type: asciidoc-sink
  source:
    url: https://confluence.corp.domain
    username: ci-aa8s77sf
    password: dfghsud78sfhsjd
    space: SPAC
    ancestor: 82476
```

Add to job:

```yaml
jobs:
  # ...
  plan:
  - task: produce-documentation
    config:
      ...
      outputs:
      - name: asciidoc-source
  - get: google-chrome-stable
    params:
      path: asciidoc-source
```
