# Migrate from Pre-1.0 Releases

Version 1.0 is a major release of Authcore that contains many improvements and new features. As it contains several changes that are incompatible with previous releases, developers are required to change their apps before upgrading to Version 1.0.

## Major Changes in Authcore 1.0

### API 2.0

API 2.0 is a complete revamp of Authcore API. It is a RESTful API with a simpler and more consistent design. Previous gRPC-based API was deprecated.

{% page-ref page="../api/overview.md" %}

### User Widget to change name, email and phone number has been removed

The User Widget is simplified in Authcore 1.0 to focus on security settings. Function to change name, email and phone number has been removed. Applications should implement these feature using server-to-server API calls.

{% page-ref page="../sdk/widgets.md" %}

### Admin Portal is revamped

Admin Portal \(previously, Management Web\) is completed redesigned to be more usable and functional.

![Revamped Admin Portal](../.gitbook/assets/image%20%284%29.png)

### New User Portal

Users can use a standalone User Portal to change security settings.

### Configurations

Several configuration keys and values are changed in Authcore 1.0 to become more consistent and easy-to-use.

{% page-ref page="../guides/configuration.md" %}

### Docker Deployment

Authcore can be deployed using a single Docker container. Kubernetes deployment is made easier as well.

{% page-ref page="../guides/install.md" %}

















