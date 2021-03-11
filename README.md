#    [Hexagonal architecture](https://alistair.cockburn.us/hexagonal-architecture/)

>	Create your application to work without either a UI or a database so you can run automated regression-tests against the application, work when the database becomes unavailable, and link applications together without any user involvement.

![Hexagonal architecture](https://reflectoring.io/assets/img/posts/spring-hexagonal/hexagonal-architecture.png)


An Architecturally Expressive Package Structure
----

1.	Adapters.
1.	Use Cases.
1.	Input and Output Ports.
1.	Domain Objects or entities.

![](https://reflectoring.io/assets/img/gyhdoca/dependencies.png)

```shell
#   Implementing a Use Case
mkdir -p buckpal/account..................................#	the module implementing the use cases

#   Implementing a Web Adapter
mkdir -p buckpal/account/adapter/in/web...................#	adapters/in drive our application ( interface ) 
mkdir -p buckpal/account/adapter/out/persistence..........#	adapters/out are driven by our application ( implements port/out interfaces )

#   Implementing a Persistence Adapter
mkdir -p buckpal/account/application/service..............#	use cases ( implements adapters/in )
mkdir -p buckpal/account/application/port/in..............#	incoming and outgoing ports ( interface )
mkdir -p buckpal/account/application/port/out.............#	driven by our application ( interface ) 

touch buckpal/account/domain/Account.java
touch buckpal/account/domain/Activity.java

touch buckpal/account/adapter/in/web/AccountController.java
touch buckpal/account/adapter/out/persistence/AccountPersistenceAdapter.java
touch buckpal/account/adapter/out/persistence/SpringDataAccountRepository.java

touch buckpal/account/application/port/in/SendMoneyUseCase.java
touch buckpal/account/application/port/out/LoadAccountPort.java
touch buckpal/account/application/port/out/UpdateAccountStatePort.java


package io.reflectoring.buckpal.account.domain;
public class Account {}
public class Activity {}
public class ActivityWindow {}
public class Money {}

package io.reflectoring.buckpal.account.application.port.in;
public interface SendMoneyUseCase {}
public interface GetAccountBalanceQuery {}
public class SendMoneyCommand extends SelfValidating<SendMoneyCommand> {}

package io.reflectoring.buckpal.account.application.port.out;
public interface AccountLock {}
public interface LoadAccountPort {}
public interface UpdateAccountStatePort {}

package io.reflectoring.buckpal.account.application.service;
class GetAccountBalanceService implements GetAccountBalanceQuery {}
public class MoneyTransferProperties {}
class NoOpAccountLock implements AccountLock {}
public class SendMoneyService implements SendMoneyUseCase {}
public class ThresholdExceededException extends RuntimeException {}

package io.reflectoring.buckpal.account.adapter.in.web;
class SendMoneyController {}

package io.reflectoring.buckpal.account.adapter.out.persistence;
interface ActivityRepository extends JpaRepository<ActivityJpaEntity, Long> {}
interface SpringDataAccountRepository extends JpaRepository<AccountJpaEntity, Long> {}
class AccountJpaEntity {}
class AccountMapper {}
class AccountPersistenceAdapter implements LoadAccountPort, UpdateAccountStatePort {}
class ActivityJpaEntity {}
```



Packt - Get Your Hands Dirty on Clean Architecture
----

1.	What's Wrong with Layers?
1.	Inverting Dependencies
1.	Organizing Code
1.	Implementing a Use Case
1.	Implementing a Web Adapter
1.	Implementing a Persistence Adapter
1.	Testing Architecture Elements
1.	Mapping Between Boundaries
1.	Assembling the Application
1.	Enforcing Architecture Boundaries
1.	Taking Shortcuts Consciously
1.	Deciding on an Architecture Style

>	JGiven (http://jgiven.org/ (http://jgiven.org/)), that provide a framework to create a vocabulary for your tests

>	For this reason, persistence adapter tests should run against the real database. Libraries such as Testcontainers (https://www.testcontainers.org/ (https://www.testcontainers.org/)) are a great help in this regard, spinning up a Docker container with a database on demand.
>	Mockito libary (https://site.mockito.org/ (https://site.mockito.org/)) library to create mock objects in the given...() methods.
>	A tool that supports this kind of check for Java is ArchUnit (https://github.com/TNG/ArchUnit
(https://github.com/TNG/ArchUnit)). Among other things, ArchUnit provides an API to check whether dependencies point in the expected direction. If it finds a violation, it will throw an exception. It's best run from within a test based on a unit testing framework such as JUnit, making the test fail in the event of a dependency violation.
>	HexagonalArchitecture class of the example project at https://github.com/thombergs/buckpal (https://github.com/thombergs/buckpal).
>	We should take great care to document such consciously added shortcuts, perhaps in the form of
Architecture Decision Records (ADRs) as proposed by Michael Nygard in his blog (http://thinkrelevance.com/blog/2011/11/15/documenting-architecture-decisions (http://thinkrelevance.com/blog/2011/11/15/documenting-architecture-decisions)). We owe that to our future selves and to our successors. If every member of the team is aware of this documentation, it will even reduce the Broken Windows effect, because the team will know that the shortcuts have been taken consciously and for good reason.

