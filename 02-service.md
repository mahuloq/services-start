export class AccountsService {
accounts = [
{
name: 'Master Account',
status: 'active',
},
{
name: 'Testaccount',
status: 'inactive',
},
{
name: 'Hidden Account',
status: 'unknown',
},
];
addAccount(name: string, status: string) {
this.accounts.push({ name: name, status: status });
}

updateStatus(id: number, status: string) {
this.accounts[id].status = status;
}
}

then replace text with and import

import { Component } from '@angular/core';
import { LoggingService } from '../logging.service';
import { AccountsService } from '../account.service';

@Component({
selector: 'app-new-account',
templateUrl: './new-account.component.html',
styleUrls: ['./new-account.component.css'],
providers: [LoggingService, AccountsService],
})
export class NewAccountComponent {
constructor(
private loggingService: LoggingService,
private accountsService: AccountsService
) {}

onCreateAccount(accountName: string, accountStatus: string) {
this.accountsService.addAccount(accountName, accountStatus);
this.loggingService.logStatusChange(accountStatus);
}
}

Howevor, services work on a Heirarchy and provide it to every child, so we messed up putting it in appcomponent and its children.

to solve, just remove it from the providers in the children as it recieves it from the parent

@Component({
selector: 'app-new-account',
templateUrl: './new-account.component.html',
styleUrls: ['./new-account.component.css'],
providers: [LoggingService],
})

---

You can also inject a service into a service
import { Injectable } from '@angular/core';
import { LoggingService } from './logging.service';

@Injectable()
export class AccountsService {
accounts = [
{
name: 'Master Account',
status: 'active',
},
{
name: 'Testaccount',
status: 'inactive',
},
{
name: 'Hidden Account',
status: 'unknown',
},
];
constructor(private loggingService: LoggingService) {}

addAccount(name: string, status: string) {
this.accounts.push({ name: name, status: status });
this.loggingService.logStatusChange(status);
}

updateStatus(id: number, status: string) {
this.accounts[id].status = status;
this.loggingService.logStatusChange(status);
}
}

allows account to use the logging service, remember to @Injectable()
