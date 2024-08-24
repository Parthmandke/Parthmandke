from django.db import models
from django.contrib.auth.models import User

class Customer(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    address = models.CharField(max_length=255)
    account_number = models.CharField(max_length=50)

class ServiceRequest(models.Model):
    SERVICE_TYPES = (
        ('installation', 'Installation'),
        ('repair', 'Repair'),
        ('maintenance', 'Maintenance'),
    )
    STATUS_CHOICES = (
        ('pending', 'Pending'),
        ('in_progress', 'In Progress'),
        ('resolved', 'Resolved'),
    )
    customer = models.ForeignKey(Customer, on_delete=models.CASCADE)
    service_type = models.CharField(choices=SERVICE_TYPES, max_length=50)
    description = models.TextField()
    status = models.CharField(choices=STATUS_CHOICES, default='pending', max_length=50)
    submission_date = models.DateTimeField(auto_now_add=True)
    resolution_date = models.DateTimeField(null=True, blank=True)
    attachment = models.FileField(upload_to='attachments/', null=True, blank=True)

class SupportRepresentative(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    name = models.CharField(max_length=100)
    email = models.EmailField()

class RequestUpdate(models.Model):
    service_request = models.ForeignKey(ServiceRequest, on_delete=models.CASCADE, related_name='updates')
    update_date = models.DateTimeField(auto_now_add=True)
    update_description = models.TextField()
