<template>
    <div>
        <h2 id="page-heading">
            <span v-text="$t('kbaseApp.paso.home.title')" id="paso-heading">Pasos</span>
            <router-link :to="{name: 'PasoCreate'}" tag="button" id="jh-create-entity" class="btn btn-primary float-right jh-create-entity create-paso">
                <font-awesome-icon icon="plus"></font-awesome-icon>
                <span  v-text="$t('kbaseApp.paso.home.createLabel')">
                    Create a new Paso
                </span>
            </router-link>
        </h2>
        <b-alert :show="dismissCountDown"
            dismissible
            :variant="alertType"
            @dismissed="dismissCountDown=0"
            @dismiss-count-down="countDownChanged">
            {{alertMessage}}
        </b-alert>
        <br/>
        <div class="alert alert-warning" v-if="!isFetching && pasos && pasos.length === 0">
            <span v-text="$t('kbaseApp.paso.home.notFound')">No pasos found</span>
        </div>
        <div class="table-responsive" v-if="pasos && pasos.length > 0">
            <table class="table table-striped">
                <thead>
                <tr>
                    <th v-on:click="changeOrder('id')"><span v-text="$t('global.field.id')">ID</span> <font-awesome-icon icon="sort"></font-awesome-icon></th>
                    <th v-on:click="changeOrder('paso')"><span v-text="$t('kbaseApp.paso.paso')">Paso</span> <font-awesome-icon icon="sort"></font-awesome-icon></th>
                    <th v-on:click="changeOrder('desc')"><span v-text="$t('kbaseApp.paso.desc')">Desc</span> <font-awesome-icon icon="sort"></font-awesome-icon></th>
                    <th v-on:click="changeOrder('instruccionId')"><span v-text="$t('kbaseApp.paso.instruccion')">Instruccion</span> <font-awesome-icon icon="sort"></font-awesome-icon></th>
                    <th></th>
                </tr>
                </thead>
                <tbody>
                <tr v-for="paso in pasos"
                    :key="paso.id">
                    <td>
                        <router-link :to="{name: 'PasoView', params: {pasoId: paso.id}}">{{paso.id}}</router-link>
                    </td>
                    <td>{{paso.paso}}</td>
                    <td>{{paso.desc}}</td>
                    <td>
                        <div v-if="paso.instruccionId">
                            <router-link :to="{name: 'InstruccionView', params: {instruccionId: paso.instruccionId}}">{{paso.instruccionId}}</router-link>
                        </div>
                    </td>
                    <td class="text-right">
                        <div class="btn-group">
                            <router-link :to="{name: 'PasoView', params: {pasoId: paso.id}}" tag="button" class="btn btn-info btn-sm details">
                                <font-awesome-icon icon="eye"></font-awesome-icon>
                                <span class="d-none d-md-inline" v-text="$t('entity.action.view')">View</span>
                            </router-link>
                            <router-link :to="{name: 'PasoEdit', params: {pasoId: paso.id}}"  tag="button" class="btn btn-primary btn-sm edit">
                                <font-awesome-icon icon="pencil-alt"></font-awesome-icon>
                                <span class="d-none d-md-inline" v-text="$t('entity.action.edit')">Edit</span>
                            </router-link>
                            <b-button v-on:click="prepareRemove(paso)"
                                   variant="danger"
                                   class="btn btn-sm"
                                   v-b-modal.removeEntity>
                                <font-awesome-icon icon="times"></font-awesome-icon>
                                <span class="d-none d-md-inline" v-text="$t('entity.action.delete')">Delete</span>
                            </b-button>
                        </div>
                    </td>
                </tr>
                </tbody>
            </table>
        </div>
        <b-modal ref="removeEntity" id="removeEntity" >
            <span slot="modal-title"><span id="kbaseApp.paso.delete.question" v-text="$t('entity.delete.title')">Confirm delete operation</span></span>
            <div class="modal-body">
                <p id="jhi-delete-paso-heading" v-bind:title="$t('kbaseApp.paso.delete.question')">Are you sure you want to delete this Paso?</p>
            </div>
            <div slot="modal-footer">
                <button type="button" class="btn btn-secondary" v-text="$t('entity.action.cancel')" v-on:click="closeDialog()">Cancel</button>
                <button type="button" class="btn btn-primary" id="jhi-confirm-delete-paso" v-text="$t('entity.action.delete')" v-on:click="removePaso()">Delete</button>
            </div>
        </b-modal>
        <div v-show="pasos && pasos.length > 0">
            <div class="row justify-content-center">
                <jhi-item-count :page="page" :total="queryCount" :itemsPerPage="itemsPerPage"></jhi-item-count>
            </div>
            <div class="row justify-content-center">
                <b-pagination size="md" :total-rows="totalItems" v-model="page" :per-page="itemsPerPage" :change="loadPage(page)"></b-pagination>
            </div>
        </div>
    </div>
</template>

<script lang="ts" src="./paso.component.ts">
</script>
