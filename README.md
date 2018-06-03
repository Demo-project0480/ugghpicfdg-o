job-template:
    name: '{project-name}-{stream}-verify-java'

    project-type: freestyle
    concurrent: true
    node: '{build-node}'

    properties:
      - ecomp-infra-properties:
          build-days-to-keep: '{build-days-to-keep}'

    parameters:
      - ecomp-infra-parameters:
          project: '{project}'
          branch: '{branch}'
          refspec: 'refs/heads/{branch}'
          artifacts: '{archive-artifacts}'

    scm:
      - gerrit-trigger-scm:
          refspec: '$GERRIT_REFSPEC'
          choosing-strategy: 'gerrit'

    wrappers:
      - ecomp-infra-wrappers:
          build-timeout: '{build-timeout}'

    triggers:
      - gerrit-trigger-patch-submitted:
          server: '{server-name}'
          project: '{project}'
          branch: '{branch}'
          files: '**'

    builders:
      - provide-maven-settings:
          global-settings-file: 'global-settings'
          settings-file: '{mvn-settings}'
      - maven-target:
          maven-version: 'mvn33'
          goals: 'clean install'
          settings: '{mvn-settings}'
          settings-type: cfp
          global-settings: 'global-settings'
          global-settings-type: cfp
