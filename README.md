<p align="center"><img src="assets/icon.png" width="128" height="128" alt="Eturlia Compat: Sable icon"></p>

<h1 align="center">Eturlia Compat: Sable</h1>

<p align="center">Design sketch of a NeoForge 1.21.1 module that would let the Sable physics mod and Create Aeronautics run on Eturlia, a Folia-based regionized server core. Not functional.</p>

<p align="center">
  <a href="https://github.com/milkycloud-dev/eturlia-compat-sable/actions/workflows/build.yml"><img src="https://github.com/milkycloud-dev/eturlia-compat-sable/actions/workflows/build.yml/badge.svg" alt="Build"></a>
  <a href="https://github.com/milkycloud-dev/eturlia-compat-sable/releases/latest"><img src="https://img.shields.io/github/v/release/milkycloud-dev/eturlia-compat-sable" alt="Release"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-proprietary-lightgrey" alt="License: proprietary"></a>
</p>

<p align="center"><a href="#english">English</a> | <a href="#русский">Русский</a></p>

<a id="english"></a>

## English

### Status

**Design sketch, no effect in game.** Every handler is an empty stub and no mixins are applied. Installing the jar only adds an entry to the mod list and a few log lines.

This module comes from [eturlia-core](https://github.com/milkycloud-dev/eturlia-core), our Folia + NeoForge server core. It belongs to an early approach where every incompatible mod got its own compat module. On 2026-08-11 the project switched to the opposite rule: the core absorbs incompatibilities and mods stay untouched. Since then eturlia-core has not built or shipped this module. It is published separately as a record of the design and as a starting point if the per-mod approach is ever needed again.

### Intended design

On Folia every region of the world is ticked by its own thread, and world data may only be touched from the thread that owns it. The module was meant to cover the places where Sable and Create Aeronautics cross those borders:

- **Sub-level ownership**: move a physics sub-level to the new owning region when a vehicle crosses a region border (`CrossRegionSubLevelManager`).
- **Physics bridge**: pass results of the Rapier physics step to region threads through queues instead of touching the world from the physics thread (`SablePhysicsRegionBridge`).
- **JNI audit**: detect callbacks from the native physics engine that access Minecraft data from the wrong thread (`JNIThreadSafetyAuditor`, modes WARN, STRICT, SILENT).
- **Vehicle assembly**: read and place blocks of a large vehicle through the regions that own them (`VehicleAssemblyRegionHandler`).

### Requirements

- Minecraft 1.21.1, NeoForge 21.1
- Sable and Create Aeronautics (declared as required dependencies)
- An Eturlia core for the design to make sense; a normal NeoForge server has no regions to bridge
- Java 21

### Releases

Every tag `v*` is built by GitHub Actions from this source and published in [Releases](../../releases). The build log of each release is public, so every jar can be traced to its commit. The release jar exists so the build stays verified; it is not meant for production use.

### Known limitations

- Nothing is implemented, see Status.
- Compile-time coordinates of the target mods were never pinned, so the handlers compile against NeoForge only and cannot reference mod classes yet.
- The mixin configuration was removed on purpose: it referenced classes that do not exist and crashed mod loading.

### License

Proprietary, all rights reserved. Official release binaries may be run unmodified on servers you operate. Copying, modifying or redistributing the code or the binaries requires written permission. Full terms: [LICENSE](LICENSE). Earlier copies of this code inside eturlia-core were published under LGPL-2.1-or-later and stay under that license.

<a id="русский"></a>

## Русский

### Статус

**Эскиз, в игре ничего не делает.** Все обработчики пустые, миксины не применяются. Установленный jar только появляется в списке модов и пишет несколько строк в лог.

Модуль взят из [eturlia-core](https://github.com/milkycloud-dev/eturlia-core), нашего серверного ядра на Folia + NeoForge. Он относится к раннему подходу, когда под каждый несовместимый мод делался свой модуль совместимости. 11.08.2026 проект перешёл на обратное правило: несовместимость поглощает ядро, а моды не трогаются. С тех пор eturlia-core этот модуль не собирает и не поставляет. Здесь он опубликован отдельно как запись замысла и как отправная точка, если подход «модуль на мод» когда-нибудь снова понадобится.

### Замысел

На Folia каждый регион мира тикает в своём потоке, и трогать данные мира можно только из потока-владельца. Модуль должен был закрыть места, где Sable и Create Aeronautics пересекают эти границы:

- **Владение подуровнями**: переносить физический подуровень в новый регион, когда аппарат пересекает границу региона (`CrossRegionSubLevelManager`).
- **Мост физики**: передавать результаты шага физики Rapier потокам регионов через очереди, а не трогать мир из потока физики (`SablePhysicsRegionBridge`).
- **Аудит JNI**: ловить вызовы из нативного физического движка, которые лезут в данные Minecraft не из того потока (`JNIThreadSafetyAuditor`, режимы WARN, STRICT, SILENT).
- **Сборка аппаратов**: читать и ставить блоки большого аппарата через регионы, которым они принадлежат (`VehicleAssemblyRegionHandler`).

### Требования

- Minecraft 1.21.1, NeoForge 21.1
- Sable и Create Aeronautics (объявлены обязательными зависимостями)
- Ядро Eturlia, иначе замысел не имеет смысла: на обычном сервере NeoForge нет регионов, которые нужно связывать
- Java 21

### Релизы

Каждый тег `v*` собирается из этих исходников в GitHub Actions и публикуется в [Releases](../../releases). Лог сборки каждого релиза открыт, так что любой jar можно сверить с его коммитом. Jar релиза нужен, чтобы сборка оставалась проверенной; для работы на сервере он не предназначен.

### Известные ограничения

- Ничего не реализовано, см. «Статус».
- Координаты целевых модов для компиляции так и не были закреплены, поэтому обработчики собираются только против NeoForge и пока не могут ссылаться на классы модов.
- Конфигурация миксинов убрана намеренно: она ссылалась на несуществующие классы и роняла загрузку модов.

### Лицензия

Проприетарная, все права защищены. Официальные сборки из релизов можно запускать без изменений на своих серверах. Копировать, изменять и распространять код или сборки можно только с письменного разрешения. Полный текст: [LICENSE](LICENSE). Прежние копии этого кода внутри eturlia-core выходили под LGPL-2.1-or-later и остаются под ней.
