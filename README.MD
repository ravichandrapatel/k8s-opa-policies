# Gatekeeper Policy Library

Welcome to the Gatekeeper Policy Library! This repository contains a set of pre-defined policies designed to enhance security and enforce best practices within Kubernetes clusters.

## Policy Organization

The policies in this library are organized into three main categories:

1. **General Policies**: These policies focus on setting cluster-wide rules and restrictions to ensure consistent security measures across all resources.

2. **Pod Security Policies**: These policies are specifically tailored to define and enforce security standards for pods, ensuring that each pod's security context meets the desired configuration.

3. **Test Cases**: The test cases directory contains sample configurations and test suites to validate the effectiveness of the policies.


## Contents of Each Policy Subdirectory

Each policy subdirectory contains the following components:

- **ConstraintTemplate.yaml**: This file defines the structure and parameters of a policy, serving as a template for creating specific constraints.

- **Constraint**: An instance of a policy defined by a ConstraintTemplate. It specifies the parameters and values necessary to enforce the policy on Kubernetes objects.

## Contents of the Test Cases Subdirectory

The test cases subdirectory includes the following:

- **Example YAMLs**: These files demonstrate how to use the policies with sample YAML configurations, making it easier to understand how the policies work.

- **suit.yaml**: This file contains test cases used by Gator, a tool for validating policies, to ensure that the policies are correctly implemented and functioning as expected.

## General Policies

1. **Allowed Repositories**: Ensures that only approved container image repositories are used, reducing the risk of deploying images from untrusted sources.

2. **Allowed Storage Class**: Defines the allowed storage classes for Persistent Volume Claims (PVCs) and Storage Solutions (SS), helping manage storage resources efficiently.

3. **Block Container With No Resource**: Enforces the specification of resource requests and limits for containers to optimize resource utilization and stability.

4. **Block Latest Tag**: Prevents the use of the latest tag for container images to ensure consistent and predictable deployments.

5. **Block NodePort**: Disallows the use of NodePort services to minimize the exposure of services to external networks.

6. **Block Services With Type LoadBalancer**: Blocks services with type LoadBalancer to reduce the attack surface and limit external access to services.

7. **Block Wildcard Ingress**: Disallows wildcard hosts in Ingress configurations to prevent unintended exposure and potential security vulnerabilities.

8. **Disallow Automount Service Account Token for Pod**: Prevents automatic mounting of service account tokens in Pods to reduce the risk of token theft or misuse.

## Pod Security Policy Policies

1. **Allowed Capabilities**: Specifies the allowed capabilities for containers, restricting potentially dangerous operations and enhancing security.

2. **Allow Read Only Root Filesystem**: Controls whether containers are allowed to have a read-only root filesystem to mitigate the impact of filesystem attacks.

3. **Allow Seccomp**: Determines whether containers are allowed to use Seccomp profiles, which restrict syscall access for additional security.

4. **Block Allow Privilege Escalation**: Prevents containers from allowing privilege escalation to mitigate the risk of unauthorized access to host resources.

5. **Block Empty Dir**: Prohibits the use of emptyDir volumes in containers to reduce the risk of data loss or unauthorized access.

6. **Block Forbidden Sysctls**: Blocks the use of forbidden sysctls in containers to prevent potentially unsafe kernel modifications.

7. **Block Host Filesystem**: Prevents containers from using the host filesystem to reduce the risk of unauthorized access.

8. **Block Host Namespace**: Blocks containers from accessing the host namespace to enhance isolation and security.

9. **Block Privileged Container**: Prevents containers from running in privileged mode to limit their privileges and reduce the attack surface.

10. **Block Privileged Ports**: Prevents non-privileged containers from binding to system ports to enhance security.

11. **Block Proc Mount**: Restricts access to system information through the proc filesystem to enhance security.

12. **SELinux**: Specifies SELinux configurations for containers to enforce mandatory access controls and enhance security.

Feel free to explore each policy and use them to ensure the security and stability of your Kubernetes environment!

## Testing with Gator

This section describes how to use Gator, a tool specifically designed for validating Gatekeeper policies, to test the effectiveness of the policies within this library.

### Prerequisites

Before testing with Gator, ensure the following prerequisites are met:

- **Kubernetes Cluster with Gatekeeper**: You should have a Kubernetes cluster set up with Gatekeeper installed and properly configured.

- **Installation of Gator CLI**: Install the gator CLI tool on your local machine. You can find installation instructions on the Gatekeeper project website: [Gator Installation Instructions](https://open-policy-agent.github.io/gatekeeper/website/docs/)

### Testing Workflow

Follow these steps to test the policies using Gator:

1. **Choose a Policy**: Select the policy you want to test from the available directories within the repository.

2. **Review Test Suite (suite.yaml)**: Each policy directory likely contains a suite.yaml file. This file defines a test suite using Gator to validate the policy's functionality. Review this file to understand the test cases that will be executed.

3. **Run the Test Suite**: Navigate to the directory containing the policy you want to test and execute the following command to run the test suite defined in suite.yaml:

    ```bash
    gator verify TestCases/...
    ```

    **Note**: Use this command with caution as it instructs Gator to execute the test cases defined in the suite.yaml file.

4. **Interpret the Results**: Gator will generate test results indicating whether each test case passed or failed. Passing test cases demonstrate that the policy functions as intended. Failing test cases might indicate issues with the policy configuration or unexpected behavior.

5. **Analyze Failures**: If any test cases fail, carefully review the output from Gator and the corresponding policy files to identify the root cause of the failure. This might involve debugging the policy configuration or adjusting the test cases to accurately reflect the intended behavior.

### Additional Notes

- **Test Cases Directory**: The suite.yaml file might contain references to other YAML files defining specific test case scenarios. These files might be located within the same directory as the policy or in a separate test cases directory within the repository.

- **Utilizing gator explain**: You can leverage the `gator explain` command to gain more detailed information about specific policies and their expected behavior. Refer to the Gator documentation for details on this command and other functionalities.

## Testing Policies with Test Cases

To test the policies using the provided test cases, navigate to the testcases directory within the repository. Each policy's test cases are organized into separate directories. Follow the same testing workflow outlined above, but instead of navigating to the policy directory, navigate to the corresponding test cases directory and execute the `gator test` command with the appropriate suite.yaml file.

By incorporating this testing section into your README, you provide a valuable guide for users to validate the functionality of the policies within your Gatekeeper policy library. This empowers users to confidently deploy and rely on these policies to enhance the security posture of their Kubernetes clusters.

